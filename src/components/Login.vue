<template>
    <div class="login_contrain">
        <div class="login_box">
            <div class="avag_login">
                <img src="../assets/logo.png" alt="">
            </div>
            <el-form class="login_form" :model="loginForm" :rules="loginRules" ref="loginFormRef">
                <el-form-item prop="username">
                    <el-input prefix-icon="el-icon-user" v-model="loginForm.username"></el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input prefix-icon="el-icon-key" v-model="loginForm.password" type="password"></el-input>
                </el-form-item>
                <div class="btns">
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="reset">重置</el-button>
                </div>
            </el-form>

        </div>
    </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginRules: {
        username: [
          {
            required: true,
            message: '请输入用户名',
            trigger: 'blur'
          },
          {
            min: 3,
            max: 10,
            message: '长度在 3 到 10 个字符',
            trigger: 'blur'
          }
        ],
        password: [
          {
            required: true,
            message: '请输入密码',
            trigger: 'blur'
          },
          {
            min: 6,
            max: 16,
            message: '长度在 6 到 16 个字符',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    reset () {
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) {
          return this.$message.error({
            showClose: true,
            message: '登录失败'
          })
        }
        this.$message.success({
          showClose: true,
          message: '登录成功'
        })
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style scoped lang="less">
    .login_contrain {
        width: 100%;
        height: 100%;
        background-color: #2b4b6b;

        .login_box {
            background-color: #fff;
            width: 480px;
            height: 300px;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            border-radius: 3px;

            .avag_login {
                width: 140px;
                height: 140px;
                position: absolute;
                left: 50%;
                transform: translate(-50%, -50%);
                border: 1px solid #eee;
                border-radius: 50%;
                padding: 10px;
                background-color: #fff;
                box-shadow: 0 0 10px #eee;

                img {
                    width: 100%;
                    height: 100%;
                    border-radius: 50%;
                    background-color: #eee;
                }
            }

            .login_form {
                position: absolute;
                bottom: 20px;
                width: 100%;
                padding: 0 20px;
                box-sizing: border-box;

                .btns {
                    display: flex;
                    justify-content: flex-end;
                }
            }
        }
    }
</style>
