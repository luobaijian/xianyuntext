<template>
  <div class="container">
    <el-row type="flex" justify="space-between">
      <!-- 订单表单 -->
      <div class="main">
        <OrderForm :data='infoData'/>
      </div>

      <!-- 侧边栏 -->
      <div class="aside"></div>
    </el-row>
  </div>
</template>

<script>
import OrderForm from '@/components/air/orderForm'
export default {
  components: {
    OrderForm
  },
  data () {
    return {
      infoData:{
        insuranceInfo:[],
      }
    }
  },
  mounted () {
    const {query}=this.$route;
    this.$axios({
      url:`/airs/${query.id}`,
      params:{
        seat_xid:query.seat_xid
      }
    })
    .then(res=>{
      this.infoData=res.data
      this.infoData.insuranceInfo=res.data.insurances
    })

  }
};
</script>

<style lang="less" scoped>
.container {
  width: 1000px;
  margin: 20px auto;
}

/*aside*/
.aside {
  width: 350px;
  height: fit-content;
  border: 1px #ddd solid;
}
</style>