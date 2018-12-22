<template>
  <div id="login">
    <el-form ref="form" :model="form" :rules="rules" label-width="80px">
      <img src="../assets/avatar.jpg" alt>
      <el-form-item label="用户名" prop="username">
        <el-input v-model="form.username"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" v-model="form.password" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">登录</el-button>
        <el-button @click="onRest">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      form: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 9, message: '长度在 3 到 9 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    onRest() {
      this.$refs.form.resetFields()
    },
    onSubmit() {
      this.$refs.form.validate(valid => {
        if (valid) {
          axios({
            method: 'post',
            url: 'http://localhost:8888/api/private/v1/login',
            data: this.form
          }).then(res => {
            if (res.data.meta.status === 200) {
              this.$message.success('恭喜你，登陆成功')
              localStorage.setItem('token', res.data.data.token)
              this.$router.push('/home')
            } else if (res.data.meta.status === 400) {
              this.$message.error('错了哦，登录失败')
            }
          })
        } else {
          return false
        }
      })
    }
  }
}
</script>
<style lang="less">
#app {
  background: #2d434c;
  overflow: hidden;

  #login {
    width: 400px;
    height: 200px;
    margin: 200px auto;
    background: #fff;
    border-radius: 15px;
    padding: 50px 40px 20px 10px;
    position: relative;

    img {
      width: 120px;
      height: 120px;
      position: absolute;
      left: 50%;
      top: -90px;
      transform: translateX(-50%);
      border-radius: 50%;
    }
  }
}
</style>
