<template>
  <div class="container">
    <div class="block"></div>
    <!-- 轮播图 -->
    <el-carousel :interval="5000" arrow="always" height="700px">
      <el-carousel-item v-for="(item,index) in banners" :key="index">
        <div
          class="banner-pic"
          :style="`background:url(${$axios.defaults.baseURL}${item.url}) center center no-repeat;
            background-size:contain contain`"
        ></div>
      </el-carousel-item>
    </el-carousel>
    <!-- 轮播图里面的搜索框 -->
    <div class="banner-content">
      <!-- tab栏 -->
      <div class="search-tab">
        <el-row type="flex" align="middle">
          <span
            v-for="(item,index) in options"
            :key="index"
            :class="{active:index===currentOption}"
            @click="handleClick(index)"
          ><i>{{item.name}}</i></span>
        </el-row>
      </div>
      <!-- 输入框 -->
      <div class="search-input">
        <el-input
          :placeholder="options[currentOption].placeholder"
          class="input-with-select"
          v-model="inputSearch"
          @keyup.enter="handleSearch"
        >
          <el-button slot="append" icon="el-icon-search" @click="handleSearch"></el-button>
        </el-input>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputSearch: "",
      currentOption: 0,
      banners: [],
      options: [
        {
          name: "攻略",
          placeholder: "搜索城市",
          pageUrl: "/post?city="
        },
        {
          name: "酒店",
          placeholder: "搜索酒店",
          pageUrl: "/post?hotel="
        },
        {
          name: "机票",
          placeholder: "搜索机票",
          pageUrl: "/air"
        }
      ]
    };
  },
  methods: {
    handleSearch() {},
    handleClick(index) {
      this.currentOption = index;
    }
  },
  mounted() {
    this.$axios({
      url: "/scenics/banners"
    })
      .then(res => {
        console.log(res);
        this.banners = res.data.data;
      })
      .catch(err => {
        console.log(err);
      });
  }
};
</script>

<style scoped lang="less">
.container{
    min-width:1000px;
    margin:0 auto;
    position:relative;

    /deep/ .el-carousel__container{
        height:700px;
    }

    .banner-pic{
        width:100%;
        height:100%;
    }

    .banner-content{
        z-index:9;
        width:1000px;
        position:absolute;
        left:50%;
        top:45%;
        margin-left: -280px;
        border-top:1px transparent solid;

        .search-bar{
            width:552px;
            margin:0 auto;
        }

        .search-tab{
            .active{
                i{
                color:#333;
                }
                &::after{
                background: #eee;
                }
            }

            span{
                width:82px;
                height:36px;
                display:block;
                position: relative;
                margin-right:8px;
                cursor: pointer;

                i{
                position:absolute;
                z-index:2;
                display: block;
                width:100%;
                height:100%;
                line-height:30px;
                text-align:center;
                color:#fff;
                }

                &:after{
                position: absolute;
                left:0;
                top:0;
                display:block;
                content: "";
                width:100%;
                height:100%;
                border: 1px rgba(255,255,255,.2) solid;
                border-bottom: none;
                transform: scale(1.1,1.3) perspective(.7em) rotateX(2.2deg);
                transform-origin: bottom left;
                background: rgba(0,0,0,.5);
                border-radius:1px 2px 0 0;
                box-sizing:border-box;
                }
            }
        }

        .search-input{
            width:550px;
            height:46px;
            background:#fff;
            border-radius: 0 4px 4px 4px;
            border: 1px rgba(255,255,255,.2) solid;
            border-top:none;
            box-sizing: unset;

            input{
                flex:1;
                height:20px;
                padding: 13px 15px;
                outline: none;
                border:0;
                font-size:16px;
            }

            .el-icon-search{
                cursor :pointer;
                font-size:22px;
                padding:0 10px;
                font-weight:bold;
            }
        }
    }
}
</style>