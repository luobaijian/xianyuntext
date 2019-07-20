<template>
  <div class="container">
    <el-row type="flex" align="middle" justify="content" class="main">
      <div class="form-wrapper">
        <el-row type="flex" justify="content" class="tabs">
          <span
            :class="{active:tabCurrent===index}"
            v-for="(item,index) in ['登录','注册']"
            :key="item.index"
            @click="handleClickTab(index)"
          >{{item}}</span>
        </el-row>
        <el-form :model="loginForm" status-icon :rules="rules" ref="ruleForm" class="demo-ruleForm">
          <el-form-item prop="username" class="userInfo">
            <el-input
              type="text"
              v-model="loginForm.username"
              autocomplete="off"
              placeholder="请输入账号"
            ></el-input>
          </el-form-item>
          <el-form-item prop="password" class="userInfo">
            <el-input
              type="password"
              v-model="loginForm.password"
              autocomplete="off"
              placeholder="请输入密码"
            ></el-input>
          </el-form-item>
        </el-form>
        <p class="form-text">
          <nuxt-link to="#">忘记密码</nuxt-link>
        </p>
        <el-button class="submit" type="primary" @click="handleLoginSubmit">登录</el-button>
      </div>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      rules: {
        username: [
          {
            required: true,
            message: "请输入用户名",
            trigger: "blur"
          }
        ],
        password: [
          {
            required: true,
            message: "请输入密码",
            trigger: "blur"
          }
        ]
      },
      loginForm: {
        username: "",
        password: ""
      },
      tabCurrent: 0
    };
  },
  methods: {
    handleClickTab(index) {
      this.tabCurrent = index;
    },
    handleLoginSubmit(){
      this.$refs['ruleForm'].validate((valid)=>{
        if(valid){
          this.$axios({
            method:'POST',
            url:'/accounts/login',
            data:this.loginForm
          }).then(res=>{
            console.log(res.data)
            this.$message({
              type:'success',
              message:'登陆成功'
            })
          })
          .catch(err=>{
            console.log(err);
          })
        }
      })
    }
  }
};
</script>

<style lang="less" scoped>
.container {
  background: url(http://157.122.54.189:9095/assets/images/th03.jfif) center 0;
  height: 700px;
  min-width: 1000px;
  .main {
    width: 1000px;
    height: 100%;
    margin: 0 auto;
    position: relative;
    .form-wrapper {
      width: 400px;
      height: 282px;
      border: 1px solid #ccc;
      background-color: #eee;
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      .tabs {
        span {
          display: inline-block;
          width: 50%;
          height: 50px;
          margin-top: 2px solid #eee;
          box-sizing: border-box;
          color: 16px;
          background: #ccc;
          text-align: center;
          line-height: 48px;
          cursor: pointer;
          &.active {
            color: orange;
            border-top: 2px solid orange;
            background: #eee;
            flex-wrap: bold;
          }
        }
      }
      .userInfo {
        width: 350px;
        margin: 20px auto;
      }
      .form-text {
        font-size: 12px;
        color: #409eff;
        text-align: right;
        line-height: 1;
        margin-right: 25px;
      }
      .submit{
        width:350px;;
        margin-left:25px;
        margin-top: 15px;
    }
    }
  }
}
</style>

