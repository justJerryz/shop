<template>
  <el-container class="home">
    <el-header class="title">
      <div class="logo"></div>
      <h1>后台管理系统</h1>
      <div class="logout">
        <a href="javascript:;" @click="logout">退出</a>
        <span>欢迎光临</span>
      </div>
    </el-header>
    <el-container class="main">
      <el-aside width="200px" class="aside">
        <el-menu
          default-active="2"
          class="el-menu-vertical-demo"
          background-color="#545c64"
          text-color="#fff"
          active-text-color="#ffd04b"
          unique-opened
          router
        >
          <el-submenu index="1">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>用户管理</span>
            </template>
            <el-menu-item index="/user">
              <i class="el-icon-menu"></i>
              <span slot="title">用户列表</span>
            </el-menu-item>
          </el-submenu>
          <el-submenu index="2">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>权限管理</span>
            </template>
            <el-menu-item index="2-1">
              <i class="el-icon-menu"></i>
              <span slot="title">角色列表</span>
            </el-menu-item>
            <el-menu-item index="2-2">
              <i class="el-icon-menu"></i>
              <span slot="title">权限列表</span>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>
<script>
export default {
  methods: {
    logout() {
      this.$confirm('此操作将退出登录, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
        .then(() => {
          localStorage.removeItem('token')
          this.$router.push('/login')
          this.$message.success('退出成功')
        })
        .catch(() => {
          this.$message({
            type: 'info',
            message: '已取消'
          })
        })
    }
  }
}
</script>
<style lang="less" scoped>
.home {
  height: 100%;
  .title {
    height: 60px;
    background: #b3c1cd;
    display: flex;
    justify-content: space-around;
    .logo {
      width: 180px;
      background: url('~@/assets/kbj.jpg') no-repeat center;
      background-size: cover;
    }
    h1 {
      flex: 1;
      text-align: center;
      line-height: 60px;
      color: #fff;
      font-size: 30px;
    }
    .logout {
      width: 180px;
      line-height: 60px;
      a,
      span {
        float: right;
        margin-left: 5px;
      }
      a {
        color: #e06c60;
      }
    }
  }
  .aside {
    background: #545c64;
    .el-submenu {
      width: 200px;
    }
  }
  .el-main {
    background: #eaeef1;
  }
}
</style>
