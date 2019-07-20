<template>
  <div class="container">
    <el-row type="flex" class="top" justify="space-between" align="middle">
      <el-col :span="8">单程：{{data.info.departCity}}-{{data.info.destCity}}/2019-07-17</el-col>
      <el-col :span="4">
        <el-select v-model="airport" placeholder="起飞机场">
          <el-option
            v-for="(item,index) in data.options.airport"
            :key="index"
            :label="item"
            :value="item"
          ></el-option>
        </el-select>
      </el-col>
      <el-col :span="4">
        <el-select v-model="time" placeholder="起飞时间" @change="changeTime">
          <el-option
            v-for="(item,index) in data.options.flightTimes"
            :key="index"
            :label="`${item.from}:00 - ${item.to}:00`"
            :value="`${item.from}, ${item.to}`"
          ></el-option>
        </el-select>
      </el-col>
      <el-col :span="4">
        <el-select v-model="company" placeholder="航空公司" @change="changeCompany">
          <el-option
            v-for="(item,index) in data.options.company"
            :key="index"
            :label="item"
            :value="item"
          ></el-option>
        </el-select>
      </el-col>
      <el-col :span="4">
        <el-select v-model="size" placeholder="机型" @change="changeSize">
          <el-option
            v-for="(item,index) in sizeList"
            :key="index"
            :label="item.type"
            :value="item.size"
          ></el-option>
        </el-select>
      </el-col>
    </el-row>
    <el-row class="bottom">
      筛选
      <el-button class="btn" type="success" @click="cancelFilter">撤销</el-button>
    </el-row>
  </div>
</template>
<script>
export default {
  data() {
    return {
      airport: "",
      time: "",
      company: "",
      size: "",
      sizeList: [
        { type: "大", size: "L" },
        { type: "中", size: "M" },
        { type: "小", size: "S" }
      ]
    };
  },
  props: {
    data: {
      type: Object,
      default: {}
    }
  },
  mounted() {
    console.log(this.data);
  },
  methods: {
    // 点击撤销按钮之后触发的事件
    cancelFilter(){
      //当把下列四组变为空字符串之后，由于双向绑定，使得下拉列表没有再选中任何数据，使得触发事件没有被触发
      this.airport='';
      this.time='';
      this.company='';
      this.size='';
      // 把原始数据传过去给父组件，进行原始书籍的渲染
      this.$emit('changeDataList',this.data.flights)

    },

    //选项起飞时间适合触发的事件
    changeTime(value) {
      console.log(value);
      const [from, to] = value.split(",");
      const arr = this.data.flights.filter(v => {
        const [start] = v.dep_time.split(":");
        console.log(start);
        return from <= +start && +start < to;
      });
      console.log(arr);
      this.$emit("changeDataList", arr);
    },

    //选中航空公司时候触发的事件
    changeCompany(value) {
      // console.log(value);
      const arr = this.data.flights.filter(v => {
        return v.airline_name === value;
      });
      this.$emit("changeDataList", arr);
    },

    //选中飞机机型时候触发的事件
    changeSize(value) {
      console.log(value);
      const arr = this.data.flights.filter(v => {
        return v.plane_size === value;
      });
      this.$emit("changeDataList", arr);
    }
  }
};
</script>
<style lang="less" scoped>
.container {
  margin-bottom: 30px;
}

.top {
  > div {
    /deep/ .el-select {
      margin-left: 10px;
      margin-bottom: 15px;
      height: 20px;
      line-height: 20px;
    }
  }
}
</style>

