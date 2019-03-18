<template>
  <div class="loginContainer">
     <el-card class="login-form-layout">
      <el-form autoComplete="on"
               :model="loginForm"
               :rules="loginRules"
               ref="loginForm"
               label-position="left">
        
        <h2 class="login-title color-main">Letao</h2>
        <h2 class="login-msg color-main">登录Letao，发现更多喜爱</h2>


        <el-form-item   style="margin-bottom:20px margin:0 auto;text-align:center;color: #8590A6;">
          <div class="Qrcode-content">
            <div class="Qrcode-img">
              <img width="150" height="150" src="https://img-blog.csdnimg.cn/20190314212442768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODc4OTY0MQ==,size_16,color_FFFFFF,t_70" alt="二维码">

            </div>
            <p>打开微信在「我的」页面右上角打开扫一扫</p>
          </div>
        </el-form-item>
      
    
        <el-form-item   style=" margin:0 auto;margin-bottom:40px;text-align:center;color: #8590A6;">
          <a href="#">使用密码登陆</a>
        </el-form-item>

        
      </el-form>
    </el-card>
    <img :src="login_center_bg" class="login-center-layout">
    <footer >Copyright [表情] 2018-2019 东软睿道 首页 关于 开发者平台 服务条款 隐私策略 帮助中心</footer>
  </div>
</template>

<script>
  import {isvalidUsername} from '@/utils/validate';
  import login_center_bg from '@/assets/images/login_center_bg.png'

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
        login_center_bg
      }
    },
    methods: {
      showPwd() {
        if (this.pwdType === 'password') {
          this.pwdType = ''
        } else {
          this.pwdType = 'password'
        }
      },
      

      codelogin() {
        this.$refs.loginForm.validate(valid => {
          if (valid) {
            this.loading = true;
            this.$store.dispatch('Login', this.loginForm).then(() => {
              this.loading = false;
              this.$router.push({path: '/codelogin'})
            }).catch(() => {
              this.loading = false
            })
          } else {
            console.log('参数验证不合法！');
            return false
          }
        })
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
  .login-form-layout {
    position: absolute;
    left: 0;
    right: 0;
    width: 360px;
    margin: 140px auto;
   
  }

  .login-title {
    text-align: center;
  }


  .loginContainer {
    background-image: url(https://static.zhihu.com/heifetz/assets/sign_bg.db29b0fb.png);
    background-repeat: no-repeat;
    background-color: #b8e5f8;
    background-size: cover;
    width: 100%;
    height: 100%;
    overflow: auto;
    padding-right: 15px;
}

  .login-title {
    font-size:400%;
    text-align: center;
    margin:0;
  }

  .login-center-layout {
   
    width: auto;
    height: auto;
    max-width: 100%;
    max-height: 100%;
    margin-top: 45%;
  }
  footer{
    color: white;
    position: absolute;
    bottom: 25px;
    font-size: 12px;
    display: inline-block;
    padding:0 auto;
  }
</style>
