<template>
  <div class="login-page">
    <div class="login-form">
      <el-form
        :model="loginForm"
        status-icon
        :rules="loginRules"
        ref="loginFormRef"
        label-width="100px"
        class="demo-ruleForm"
      >
        <el-form-item label="用户名" prop="username">
          <el-input v-model="loginForm.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input type="password" v-model="loginForm.password" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('loginForm')">登录</el-button>
          <el-button @click="resetForm('loginForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginRules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 4, max: 10, message: '长度在 4 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 5, max: 15, message: '长度在 5 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    submitForm(formName) {
      // console.log(this.$refs.loginFormRef.resetFields);

      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) {
          this.$message({
            showClose: true,
            message: '请输入正确的用户名和密码',
            type: 'error'
          })
          return
        }
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status != 200) {
          this.$message.error('用户名或密码错误')
        }

        window.sessionStorage.setItem('token', res.data.token)
        // console.log(window.sessionStorage.getItem('token'));

        let id = res.data.id
        // console.log(id);

        this.$router.push({ path: '/home', parmas: { id: id } })
      })
    },
    resetForm(formName) {
      this.$refs.loginFormRef.resetFields()
    }
  }
}
</script>

<style lang="less" scoped>
.login-page {
  height: 100%;
  background: url(http://img3.chinadaily.com.cn/static/2019-70th/img/bgn.jpg)
    no-repeat 50% 0;
}

.login-form {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 500px;
  height: 350px;
  background-color: #fff;
  transform: translate(-50%, -50%);
  border-radius: 10px;
  border: 4px solid rosybrown;
}
.el-form {
  margin-top: 95px;
}
.el-input {
  width: 345px;
}
.el-form-item__content {
  display: flex;
  justify-content: space-around;
}
</style>