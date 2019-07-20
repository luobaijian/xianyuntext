<template>
  <div class="search-form">
    <!-- 头部tab切换 -->
    <el-row type="flex" justify="content" class="search-tab">
      <span
        v-for="(item,index) in tabs"
        :key="index"
        @click="handleClickSearchTab(index)"
        :class="{active:currentTab===index}"
      >
        <i :class="item.icon"></i>
        {{item.name}}
      </span>
    </el-row>
    <!-- 表单 -->
    <el-form class="search-form-content" ref="form" label-width="80px">
      <!-- 出发城市 -->
      <el-form-item label="出发城市">
        <el-autocomplete
          :fetch-suggestions="queryDepartSearch"
          placeholder="请选择出发城市"
          @select="handleDepartSelect"
          v-model="form.departCity"
          class="el-autocomplete"
        ></el-autocomplete>
      </el-form-item>
      <!-- 到达城市 -->
      <el-form-item label="到达城市">
        <el-autocomplete
          :fetch-suggestions="queryDestSearch"
          placeholder="请选择到达城市"
          @select="handleDestSelect"
          v-model="form.destCity"
          class="el-autocomplete"
        ></el-autocomplete>
      </el-form-item>
      <!-- 出发时间 -->
      <el-form-item label="出发时间">
        <el-date-picker
          type="date"
          placeholder="请选择出发日期"
          style="width:100%"
          @change="handleDate"
          :picker-options="pickerOptions"
          v-model="form.departDate"
        ></el-date-picker>
      </el-form-item>
      <!-- 搜索按钮 -->
      <el-form-item label>
        <el-button
          style="width:100%"
          type="primary"
          icon="el-icon-search"
          @click="handleSearchSubmit"
        >搜索</el-button>
      </el-form-item>
      <!-- 切换出发和到达城市 -->
      <div class="reverse">
        <span @click="handleReverse">换</span>
      </div>
    </el-form>
    <!-- 特价机票展示区域 -->
  </div>
</template>
<script>
import moment from "moment";
export default {
  data() {
    return {
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() < Date.now();
        }
      },
      form: {
        departCity: "",
        dapartCode: "",
        destCity: "",
        destCode: "",
        departDate: ""
      },
      currentTab: 0,
      tabs: [
        { icon: "iconfont icondancheng", name: "单程" },
        { icon: "iconfont iconshuangxiang", name: "往返" }
      ]
    };
  },
  methods: {
    handleClickSearchTab(index) {
      this.currentTab = index;
    },
    // 输入了城市之后，触发的事件,返回建议的城市选项
    // value就是你输入是值
    // callback是一个回调函数
    queryDepartSearch(value, callback) {
      if (!value.trim()) {
        //不显示下拉列表
        callback([]);
        //如果没有输入搜索关键字，则不发送请求
        return;
      }
      // console.log(value);
      this.$axios({
        url: "/airs/city",
        params: {
          name: value.trim()
        }
      }).then(res => {
        // console.log(res.data);
        const { data } = res.data;
        const newData = data.map(v => {
          return {
            ...v,
            value: v.name.replace("市", "")
          };
        });
        //如果没有从下拉表中选中城市，则默认选中下拉的第一个城市
        // console.log(newData);
        this.form.departCity = newData[0].value;
        this.form.departCode = newData[0].sort;
        // 回调函数中的参数必须是一个数组
        // 数组中每一项必须是一个对象，对象中必须包含value属性
        callback(newData);
      });
    },
    queryDestSearch(value, callback) {
      if (!value.trim()) {
        //不显示下拉列表
        callback([]);
        //如果没有输入搜索关键字，则不发送请求
        return;
      }
      console.log(value);
      this.$axios({
        url: "/airs/city",
        params: {
          name: value
        }
      }).then(res => {
        console.log(res.data);
        const { data } = res.data;
        const newData = data.map(v => {
          return {
            ...v,
            value: v.name.replace("市", "")
          };
        });
        // 如果没从下拉列表中选中任何一个城市，则默认选中第一个城市
        this.form.destCity = newData[0].value;
        this.form.destCode = newData[0].sort;
        // 回调函数中的参数必须是一个数组
        // 数组中每一项必须是一个对象，对象中必须包含value属性
        callback(newData);
      });
    },
    //从下拉列表中选中出发城市后触发的事件
    // 出发这个事件时候，能够获取到选中的文字
    handleDepartSelect(item) {
      // 把选中对象值赋给表单
      this.form.departCity = item.value;
      this.form.departCode = item.sort;
    },
    // 从下拉列表中选中到达城市后触发的事件
    handleDestSelect(item) {
      this.form.destCity = item.value;
      this.form.destCode = item.sort;
    },
    // 切换出发和到达城市
    handleReverse() {
      //先把原先form的值提出来，当做定制先，再进行重新赋值
      const { departCity, departCode, destCity, destCode } = this.form;
      this.form.departCity = destCity;
      this.form.departCode = destCode;
      this.form.destCity = departCity;
      this.form.destCode = departCode;
    },
    //选择了日期之后触发的事件
    handleDate(value) {
      // console.log(value)
      this.form.departDate = moment(value).format("YYYY-MM-DD");
      console.log(this.form);
    },
    //点击搜索按钮的时候出发的事件
    handleSearchSubmit() {
      const rules = {
        departCity: { value: this.form.departCity, message: "请输入出发城市" },
        destCity: { value: this.form.destCity, message: "请输入到达城市" },
        departDate: { value: this.form.departDate, message: "请选择出发时间" }
      };
      let valid = true;
      Object.keys(rules).forEach(v => {
        //当valid变为false是时候，终止下面的代码，不再继续执行下去
        if (!valid) return;
        //如果数值为空，则显示提示信息，并且把valid这个值变为false
        if (!rules[v].value) {
          valid = false;
          //显示提示信息
          this.$message.warning(rules[v].message);
        }
      });

      if (valid) {
        this.$router.push({
          path: "/air/flights",
          query: this.form
        });
      }
      //把搜索信息添加到本地存储
      const airs=JSON.parse(localStorage.getItem('airs')||'[]')
      airs.push(this.form)
      localStorage.setItem('airs',JSON.stringify(airs))
    }
  }
};
</script>
<style lang="less" scoped>
.search-form {
  border: 1px solid #ddd;
  width: 360px;
  height: 350px;
  box-sizing: border-box;
}

.search-tab {
  span {
    display: block;
    flex: 1;
    height: 48px;
    text-align: center;
    line-height: 42px;
    box-sizing: border-box;
    background-color: #eee;
    font-weight: bold;
  }
  .active {
    border-top: 3px solod orange;
    background-color: #fff;
  }
  i {
    font-size: 18px;
  }
}
.search-form-content {
  padding: 15px 50px 15px 15px;
  position: relative;
  .el-autocomplete {
    width: 100%;
  }
}
.reverse {
  position: absolute;
  top: 35px;
  right: 15px;
  &:after,
  &:before {
    display: block;
    content: "";
    position: absolute;
    left: -35px;
    width: 25px;
    height: 1px;
    background: #ccc;
  }

  &:after {
    top: 0;
  }

  &:before {
    top: 60px;
  }

  span {
    display: block;
    position: absolute;
    top: 20px;
    right: 0;
    font-size: 12px;
    background: #999;
    color: #fff;
    width: 20px;
    height: 20px;
    line-height: 18px;
    text-align: center;
    border-radius: 2px;
    cursor: pointer;

    &:after,
    &:before {
      display: block;
      content: "";
      position: absolute;
      left: 10px;
      width: 1px;
      height: 20px;
      background: #ccc;
    }

    &:after {
      top: -20px;
    }

    &:before {
      top: 20px;
    }
  }
}
</style>

