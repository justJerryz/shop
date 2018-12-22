<template>
  <div class="user">
    <!-- 面包屑导航 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>活动列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 搜索框 -->
    <div style="width: 300px" class="search">
      <el-input placeholder="请输入内容" v-model="query" class="input-with-select">
        <el-button slot="append" icon="el-icon-search" @click="searchList"></el-button>
      </el-input>
    </div>
    <el-button type="success" plain>添加用户</el-button>
    <!-- 表格 -->
    <el-table :data="userList" style="width: 100%">
      <el-table-column prop="username" label="姓名" width="180"></el-table-column>
      <el-table-column prop="email" label="邮箱" width="180"></el-table-column>
      <el-table-column prop="mobile" label="电话"></el-table-column>
      <el-table-column prop="mg_state" label="状态">
        <template slot-scope="scope">
          <el-switch v-model="scope.row.mg_state" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button plain size="mini" type="primary" icon="el-icon-edit"></el-button>
          <el-button
            plain
            size="mini"
            type="danger"
            icon="el-icon-delete"
            @click="delData(scope.row.id)"
          ></el-button>
          <el-button plain size="mini" type="success" icon="el-icon-check">分配角色</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 分页 -->
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="[2,4,6]"
      :page-size="pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
    ></el-pagination>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      userList: [],
      query: '',
      currentPage: 1,
      pageSize: 2,
      total: 0
    }
  },
  methods: {
    render() {
      axios({
        method: 'get',
        url: 'http://localhost:8888/api/private/v1/users',
        params: {
          query: this.query,
          pagenum: this.currentPage,
          pagesize: this.pageSize
        },
        headers: {
          Authorization: localStorage.getItem('token')
        }
      }).then(res => {
        // console.log(res)
        if (res.data.meta.status === 200) {
          this.userList = res.data.data.users
          this.total = res.data.data.total
        }
      })
    },
    handleSizeChange(val) {
      // console.log(`每页 ${val} 条`)
      this.pageSize = val
      this.render()
    },
    handleCurrentChange(val) {
      // console.log(`当前页: ${val}`)
      this.currentPage = val
      this.render()
    },
    searchList() {
      this.currentPage = 1
      this.render()
    },
    delData(id) {
      this.$confirm('此操作将删除当前数据, 是否继续?', '温馨提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
        .then(() => {
          axios({
            method: 'delete',
            url: `http://localhost:8888/api/private/v1/users/${id}`,
            headers: {
              Authorization: localStorage.getItem('token')
            }
          }).then(res => {
            // console.log(res)
            if (res.data.meta.status === 200) {
              if (this.userList.length <= 1 && this.currentPage > 1) {
                this.currentPage--
              }
              this.render()
              this.$message.success('删除成功!')
            }
          })
        })
        .catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          })
        })
    }
  },
  created() {
    this.render()
  }
}
</script>
<style lang="less" scoped>
.el-breadcrumb {
  height: 50px;
  line-height: 50px;
}
.search {
  float: left;
  margin-right: 10px;
  margin-bottom: 10px;
}
</style>
