<template>
  <div class="header">
    <el-row type="flex" justify="space-between" class="main">
      <!-- logo部分 -->
      <div class="logo">
        <nuxt-link to="/">
          <img src="http://157.122.54.189:9093/images/logo.jpg" alt />
        </nuxt-link>
      </div>
      <!-- 导航菜单部分 -->
      <el-row type="flex" class="navs">
        <nuxt-link to="/">首页</nuxt-link>
        <nuxt-link to="/post">旅游攻略</nuxt-link>
        <nuxt-link to="/hotel">酒店</nuxt-link>
        <nuxt-link to="/air">国内机票</nuxt-link>
      </el-row>
      <!-- 用户，登录信息 -->
      <el-row type="flex" align="middle">
        <!-- 如果用户存在则展示用户信息（头像和昵称） -->
        <el-dropdown v-if="false">
          <el-row type="flex" align="middle" class="el-dropdown_link">
            <nuxt-link to="#">
              <img src="http://157.122.54.189:9095/assets/images/avatar.jpg" alt />
              吉山伏地魔
            </nuxt-link>
            <i class="el-icon-arrow-down el-icon--right"></i>
          </el-row>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>
              <nuxt-link to="#">个人中心</nuxt-link>
            </el-dropdown-item>
            <el-dropdown-item>
              <nuxt-link @click="handleLogout">退出</nuxt-link>
            </el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
        <!-- 不存在用户信息时候展示登录注册信息 -->
        <nuxt-link to="/user/login" class="account-link" v-else>登录 / 注册</nuxt-link>
      </el-row>
    </el-row>
  </div>
</template>
<script>
export default {
  methods: {
    handleLogout() {}
  }
};
</script>
<style lang="less" scoped>
.header {
  height: 60px;
  line-height: 60px;
  background-color: #fff;
  border-bottom: 1px solid #ddd;
  // 内减模式
  box-sizing: border-box;
  .main {
    width: 1000px;
    margin: 0 auto;
    .logo {
      width: 160px;
      padding-top: 8px;
      img {
        display: block;
        width: 100%;
      }
    }
    .navs {
      flex: 1;
      margin: 0 20px;
      a {
        display: block;
        height: 60px;
        padding: 0 10px;
        box-sizing: border-box;
        &:hover {
          color: skyblue;
          border-bottom: 5px solid skyblue;
        }
      }
      /deep/ .nuxt-link-exact-active {
        background: #409eff;
        color: #fff !important;
      }

      .message {
        height: 36px;
        line-height: 1;
        cursor: pointer;
        .el-icon-bell {
          margin-right: 2px;
          font-size: 18px;
        }
      }
      .el-dropdown-link {
        margin-left: 20px;

        &:hover {
          img {
            border-color: #409eff;
          }
        }

        a {
          display: block;
        }

        img {
          width: 32px;
          height: 32px;
          vertical-align: middle;
          border: 2px #fff solid;
          border-radius: 50px;
        }
      }

      .account-link {
        font-size: 14px;
        margin-left: 10px;
        color: #666;

        &:hover {
          color: #409eff;
          text-decoration: underline;
        }
      }
    }
  }
}
</style>

