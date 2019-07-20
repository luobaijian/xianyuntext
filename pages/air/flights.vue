<template>
  <div class="container">
    <el-row type="flex" class="content" justify="space-between">
      <!-- 顶部过滤列表 -->
      <!-- 把缓存的原始数据传递给子组件，然后子组件一个方法传给子组件，供子组件进行调用，方便最后把筛选过后的数据返回给父组件，在父组件里面进行渲染 -->
      <flightsFilters :data="cacheDataList" @changeDataList="changeDataList" />
      <!-- 侧边栏 -->
      <div class="aside">
        <flightsAside />
      </div>
    </el-row>
    <flightListsHead />
    <flightsListItem v-for="(item,index) in DataList" :key="index" :data="item" />

    <!-- 分页器 -->
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="[5,8,10,15]"
      :page-size="pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
    ></el-pagination>
  </div>
</template>
<script>
import flightListsHead from "@/components/air/flightListsHead";
import flightsListItem from "@/components/air/flightsListItem";
import flightsFilters from "@/components/air/flightsFilters";
import flightsAside from "@/components/air/flightsAside";
export default {
  components: {
    flightListsHead,
    flightsListItem,
    flightsFilters,
    flightsAside
  },
  data() {
    return {
      cacheDataList: {
        flights: {},
        info: {},
        options: {}
      },
      currentPage: 1,
      pageSize: 5,
      total: 30,
      flightsData: {
        flights: []
      },
      //当前所显示的数据
      DataList: []
    };
  },
  mounted() {
    this.$axios({
      url: "/airs",
      params: this.$route.query
    }).then(res => {
      console.log(res.data);
      this.flightsData = res.data;
      // 把请求回来的原始数据缓存一份，用于到时候传递给子组件
      this.cacheDataList = { ...res.data };
      this.setData();
    });
  },
  methods: {
    //当子组件根据相关要求筛选过滤了数据之后，把数据传递过来，由主页进行渲染
    changeDataList(arr) {
      //子组件调用了这个方法之后，把得到的arr数组重新发射给父组件，让这个数组在父组件里面进行渲染
      this.flightsData.flights = arr;
      this.setData();
    },

    //当每页要显示的条数发生变化之后
    handleSizeChange(value) {
      this.pageSize = value;
      this.setData();
    },

    //当前页发生了变化之后
    handleCurrentChange(value) {
      // console.log(value);
      this.currentPage = value;
      this.setData();   
    },

    
    setData() {
      this.DataList = this.flightsData.flights.slice(
        (this.currentPage - 1) * this.pageSize,
        this.currentPage * this.pageSize
      );
      this.currentPage = 1;
      this.total = this.flightsData.flights.length;
    }
  }
};
</script>
<style lang="less" scoped>
.container {
  width: 1000px;
  margin: 20px auto;
}
.flights-content {
  width: 745px;
  font-size: 14px;
}
.flights-content {
  width: 240px;
}
</style>
