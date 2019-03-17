<template>
  <div >
    <div class="nav">
      <ul class="nav-centent">

        <li><a href="hreftwo">管理员登录</a></li>
        <li><a href="hrefone">经销商登录</a></li>

      </ul>
    </div>
    <el-card class="login-form-layout"  v-on:mouseover.native="blu();">
      <el-form autoComplete="on"
               :model="loginForm"
               :rules="loginRules"
               ref="loginForm"
               label-position="left">
        <div style="text-align: center">
          <svg-icon icon-class="manage" style="width: 56px;height: 56px;color: #1296db"></svg-icon>
        </div>
        <h2 class="login-title color-main" style="color: #1296db">智能化跨境电商</h2>
        <div class="tab">
          <a href="#" style="height: 50px;line-height: 50px;width: 150px;text-align: center;color: #66b1ff" @click="tabId=0" class="{tabId=0}">账号密码登录</a>
          <a href="#" style="height: 50px;line-height: 50px;width: 150px;text-align: center;color: #66b1ff;float: right" @click="tabId=1" class="{tabId=1}">微信登录</a>
        </div>
        <div v-show="tabId===0" class="account">
          <el-form-item prop="username" >
            <el-input name="username"
                      type="text"
                      v-model="loginForm.username"
                      autoComplete="on"
                      placeholder="请输入用户名">
          <span slot="prefix">
            <svg-icon icon-class="user" class="color-main"></svg-icon>
          </span>
            </el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input name="password"
                      :type="pwdType"
                      @keyup.enter.native="handleLogin"
                      v-model="loginForm.password"
                      autoComplete="on"
                      placeholder="请输入密码">
          <span slot="prefix">
            <svg-icon icon-class="password" class="color-main"></svg-icon>
          </span>
              <span slot="suffix" @click="showPwd">
            <svg-icon icon-class="eye" class="color-main"></svg-icon>
          </span>
            </el-input>
          </el-form-item>
          <el-form-item style="margin-bottom: 60px">
            <el-button style="width: 100%" type="primary" :loading="loading" @click.native.prevent="handleLogin">
              登录
            </el-button>
          </el-form-item>
        </div>
        <div class="pic" v-show="tabId===1">

        </div>
      </el-form>
    </el-card>
    <img :src="login_center_bg" id="huhuhu">
    <footer >Copyright [表情] 2018-2019 东软睿道 首页 关于 开发者平台 服务条款 隐私策略 帮助中心</footer>
  </div>
</template>

<script>
  import {isvalidUsername} from '@/utils/validate';
  import login_center_bg from '@/views/login/timg1.jpg';
  import AdminLogin from '@/views/login/index';
  import AgencyLogin from '@/views/oms/apply/index.vue';


  export default {
    name: 'login',
    data() {

      const validateUsername = (rule, value, callback) => {
        if (!isvalidUsername(value)) {
          callback(new Error('请输入正确的用户名'))
        } else {
          callback()
        }
      };
      const validatePass = (rule, value, callback) => {
        if (value.length < 3) {
          callback(new Error('密码不能小于3位'))
        } else {
          callback()
        }
      };
      return {
        tabId:0,
        nowIndex:0,

        loginForm: {
          username: 'admin',
          password: '123456'
        },
        loginRules: {
          username: [{required: true, trigger: 'blur', validator: validateUsername}],
          password: [{required: true, trigger: 'blur', validator: validatePass}]
        },
        loading: false,
        pwdType: 'password',
        login_center_bg,
      }

    },
    methods: {
      hrefone:function(){
        this.$router.push({name: 'AgencyLogin'});
      },
      hreftwo:function(){
        this.$router.push({name: 'AdminLogin'});
      },
      selectStyle:function(item)
      {
        item.isSubShow=true
      },
      outStyle(item){
        item.isSubShow = false
      },
      blu(){
        //document.getElementById("huhuhu").style.WebkitFilter="blur(15px)"
        //document.getElementsByClassName("all").style.WebkitFilter="blur(15px)"
      },
      showPwd() {
        if (this.pwdType === 'password') {
          this.pwdType = ''
        } else {
          this.pwdType = 'password'
        }
      },
      handleLogin() {
        this.$refs.loginForm.validate(valid => {
          if (valid) {
            this.loading = true;
            this.$store.dispatch('Login', this.loginForm).then(() => {
              this.loading = false;
              this.$router.push({path: '/'})
            }).catch(() => {
              this.loading = false
            })
          } else {
            console.log('参数验证不合法！');
            return false
          }
        })
      }
    }
  }


</script>
<style scoped>
  [v-cloak] {
    display: none;
  }
  .login-form-layout {
    position: absolute;
    left: 0;
    right: 0;
    width: 360px;
    height: 400px;
    margin: 140px auto;
    filter:alpha(opacity=100); /*仅在ie中支持*/
    opacity:1; /*不支持ie*/

  }
  .login-title {
    text-align: center;
  }
  footer{
    color: white;
    position: absolute;
    bottom: 25px;
    font-size: 12px;
    padding-left: 560px;
  }
  .nav{width:100%;height: 50px;background: #48B246;}
  .nav .nav-centent{width: 1200px;margin:0 auto;height: 50px;}
  .nav .nav-centent  li{width: 140px;height: 50px;display: block;float: left;text-align: center;margin-right: 20px;position: relative;cursor: pointer;}
  .nav .nav-centent li a{width: 140px;height: 50px;display: block;font-size: 18px;line-height: 50px;color: #fff;}
  .nav .nav-centent ul{position: absolute;left: 0;top: 50px;z-index: 99;width: 100%;}
  .nav .nav-centent ul li{width: 140px;height: 50px;display: block;float: left;text-align: center;margin-right: 20px;line-height: 50px;background: #46B148;}
  .nav .nav-centent ul li a:hover{background: #fff;color:#48B246; }
  .nav .nav-centent li a:hover{background: #fff;color:#48B246; }
  .nav .nav-centent li a.on{background: #fff;color:#48B246; }
</style>
