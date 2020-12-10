<template>
  <div class="login_container">
    <div class="login_box">
<!--      头像区域-->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="">
      </div>
<!--      表单区域-->
      <el-form ref="loginFormRef" :rules="loginFormRules" label-width="0px" class="login_form" :model="loginForm">
<!--        用户名-->
        <el-form-item prop="username">
          <el-input placeholder="请输入用户名" v-model="loginForm.username" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
<!--        密码-->
        <el-form-item prop="password">
          <el-input placeholder="请输入密码" type="password" v-model="loginForm.password" prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
<!--        按钮-->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields();
    },
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid)
          return;
        const {data: res} =await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200)
          return this.$message.error('登录失败!')
        this.$message.success('登录成功')
        // console.log(res);
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')

      });
    }
  },
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123456',
      },
      loginFormRules: {
        // 验证用户名
        username: [
          {required: true, message: '请输入用户名', trigger: 'blur'},
          {min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur'}
        ],
        // 验证密码
        password: [
          {required: true, message: '请输入密码', trigger: 'blur'},
          {min: 6, max: 16, message: '长度在 6 到 16 个字符', trigger: 'blur'}]
      }
    }

  }

}
</script>

<style lang="less" scoped>

.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.login_container {
  background-color: #2b4b6b;
  height: 100%;
}
.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.avatar_box {
  height: 130px;
  width: 130px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;
  }
}
.btns {
  display: flex;
  justify-content: flex-end;
}
</style>