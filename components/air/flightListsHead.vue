<template>
  <el-row class="flight-title" type="flex">
    <el-col :span="5">航空信息</el-col>
    <el-col :span="14">
      <el-col :span="12">起飞时间</el-col>
      <el-col :span="12">到达时间</el-col>
    </el-col>
    <el-col :span="5">价格</el-col>
  </el-row>
</template>
<script>
export default {};
</script>
<style lang="less" scoped>
.flight-title{
  width: 100%;
  border: 1px solid #ddd;
  height: 40px;
  line-height: 40px;
  background-color: #f6f6f6;
  >div{
    text-align: center;
    font-size: 12px;
    color: #666
  }
}
</style>
