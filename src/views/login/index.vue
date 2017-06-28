<template>
    <div class="login-container">
        <canvas id="root" width="100%" height="100%"></canvas>
        <el-form autoComplete="on" :model="loginForm" :rules="loginRules" ref="loginForm" label-position="left"
                 label-width="0px"
                 class="card-box login-form">
            <h3 class="title">系统登录</h3>
            <el-form-item prop="email">
                <span class="svg-container"><wscn-icon-svg icon-class="jiedianyoujian"/></span>
                <el-input name="email" type="text" v-model="loginForm.email" autoComplete="on"
                          placeholder="邮箱"></el-input>
            </el-form-item>
            <el-form-item prop="password">
                <span class="svg-container"><wscn-icon-svg icon-class="mima"/></span>
                <el-input name="password" type="password" @keyup.enter.native="handleLogin" v-model="loginForm.password"
                          autoComplete="on" placeholder="密码"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" style="width:100%;" :loading="loading" @click.native.prevent="handleLogin">
                    登录
                </el-button>
            </el-form-item>
            <div class='tips'>admin账号为:admin@gionee.com 密码随便填</div>
            <div class='tips'>editor账号:editor@gionee.com 密码随便填</div>
        </el-form>
    </div>
</template>

<script>
    import { mapGetters } from 'vuex';
    import { isWscnEmail } from 'utils/validate';
    import CanvasAnimate from 'utils/CanvasAnimate'
    // import { getQueryObject } from 'utils';
    // import socialSign from './socialsignin';

    export default {
      components: {  },
      name: 'login',
      data() {
        const validateEmail = (rule, value, callback) => {
          if (!isWscnEmail(value)) {
            callback(new Error('请输入正确的合法邮箱'));
          } else {
            callback();
          }
        };
        const validatePass = (rule, value, callback) => {
          if (value.length < 6) {
            callback(new Error('密码不能小于6位'));
          } else {
            callback();
          }
        };
        return {
          loginForm: {
            email: 'admin@gionee.com',
            password: ''
          },
          loginRules: {
            email: [
                { required: true, trigger: 'blur', validator: validateEmail }
            ],
            password: [
                { required: true, trigger: 'blur', validator: validatePass }
            ]
          },
          loading: false,
          showDialog: false
        }
      },
      computed: {
        ...mapGetters([
          'auth_type'
        ])
      },
      methods: {
        handleLogin() {
          this.$refs.loginForm.validate(valid => {
            if (valid) {
              this.loading = true;

              this.$store.dispatch('LoginByEmail', this.loginForm).then(() => {
                //    debugger;
                this.loading = false;
                this.$router.push({ path: '/' });
                // this.showDialog = true;
              }).catch(() => {
                //    debugger;
                   this.$message({
                      message: '请输入正确的用户名',
                      type: 'error'
                    });

                // this.$message.error(err);
                this.loading = false;
              });
            } else {
              return false;
            }
          });
        },
        afterQRScan() {
          // const hash = window.location.hash.slice(1);
          // const hashObj = getQueryObject(hash);
          // const originUrl = window.location.origin;
          // history.replaceState({}, '', originUrl);
          // const codeMap = {
          //   wechat: 'code',
          //   tencent: 'code'
          // };
          // const codeName = hashObj[codeMap[this.auth_type]];
          // if (!codeName) {
          //   alert('第三方登录失败');
          // } else {
          //   this.$store.dispatch('LoginByThirdparty', codeName).then(() => {
          //     this.$router.push({ path: '/' });
          //   });
          // }
        }
      },
      created() {
        // window.addEventListener('hashchange', this.afterQRScan);
      },
      mounted(){
          let root = document.querySelector("#root")
          root.width= document.body.clientWidth;
          root.height = document.body.clientHeight;
            let a = new CanvasAnimate(root,{length:50,clicked:true,moveon:true})
            a.Run()
      },
      destroyed() {
        // window.removeEventListener('hashchange', this.afterQRScan);
      }
    }
</script>

<style rel="stylesheet/scss" lang="scss">
    @import "src/styles/mixin.scss";
    #root{
        position: absolute;
        left: 0;
        top: 0;
    }
    .tips{
      font-size: 14px;
      color: #fff;
      margin-bottom: 5px;
    }
    .login-container {
        @include relative;
        background-color: #f7fafc;
        @media (max-width:400px) {
            & {
                display: flex;
                flex: 1;
            }
        }
        input:-webkit-autofill {
            -webkit-box-shadow: 0 0 0px 1000px #293444 inset !important;
            -webkit-text-fill-color: #fff !important;
        }
        input {
            background: transparent;
            border: 0px;
            -webkit-appearance: none;
            border-radius: 0px;
            padding: 12px 5px 12px 15px;
            color: #eeeeee;
            height: 47px;
        }
        .el-input {
            display: inline-block;
            height: 47px;
            width: 85%;
        }
        .svg-container {
            padding: 6px 5px 6px 15px;
            color: #889aa4;
        }

        .title {
            font-size: 26px;
            font-weight: 400;
            color: #eeeeee;
            margin: 0px auto 40px auto;
            text-align: center;
            font-weight: bold;
        }

        .login-form {
            position: absolute;
            left: 0;
            right: 0;
            bottom: 0;
            top: 0;
            z-index: 5;
            border-radius: 10px;
            width: 400px;
            height: 400px;
            padding: 35px 35px 15px 35px;
            margin: auto;
            background-color: rgba(45,58,75,0.8);
            @media (max-width:400px) {
                & {
                    padding: 10px;
                    width: calc(100% - 20px);
                }
            }
        }

        .el-form-item {
            border: 1px solid rgba(255, 255, 255, 0.1);
            background: rgba(0, 0, 0, 0.1);
            border-radius: 5px;
            color: #454545;
        }

        .forget-pwd {
            color: #fff;
        }
    }

</style>
