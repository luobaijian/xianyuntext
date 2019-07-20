<template>
  <div id="app">
    <span v-for="(value,index) in ['新闻','体育','军事']" :key="index" @click="handleClick(index)" 
    :class="{active:current==index}">
      {{value}}
    </span>
    <ul v-if="current==0">
      <li>范冰冰分手</li>
      <li>范冰冰分手</li>
      <li>范冰冰分手</li>
    </ul>
    <ul v-if="current==1">
      <li>猛龙总冠军</li>
      <li>猛龙总冠军</li>
      <li>猛龙总冠军</li>
    </ul>
    <ul v-if="current==2">
      <li>落地98K</li>
      <li>落地98K</li>
      <li>落地98K</li>
    </ul>
  </div>
</template>
<script>
export default {
  data () {
    return {
      current:''
    }
  },
  methods: {
    handleClick(index){
      this.current=index
    }
  }
};
</script>
<style lang="less" scoped>
span{
  margin:0 10px
}
.active{
  color:red
}
*{
    margin: 0;
    padding: 0;
}
a{
    color: inherit;
    text-decoration: none
}
ul, li, ol{
    list-style: none
}
</style>

