<!-- 顶部信息栏 -->
<template>
  <header id="topbar">
    <el-row :gutter="20">
      <el-col :span="4" class="topbar-left">
        <i class="iconfont icon-kaoshi"></i>
        <span class="title" @click="index()">EXAM-SYSTEM</span>
      </el-col>
      <el-col :span="16" class="topbar-right">
        <div class="menu-icon">
          <i class="el-icon-menu" @click="setBarCollapse"></i>
        </div>
      </el-col>
      <el-col :span="3">
        <el-dropdown split-button type="primary">
          <div style="font-size: 16px; font-weight: bold">{{ user.userName }}</div>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>用户信息</el-dropdown-item>
            <el-dropdown-item>设置</el-dropdown-item>
            <el-dropdown-item>退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-col>
      <el-col :span="1">
        <i class="el-icon-user" style="padding-right: 50px"></i>
      </el-col>
    </el-row>
  </header>
</template>

<script>
import {mapState,mapMutations} from 'vuex'
import store from '@/store/store'
export default {
  data() {
    return {
      login_flag: false,
      user: { //用户信息
        userName: null,
        userId: null
      }
    }
  },
  created() {
    this.getUserInfo()
  },
  computed: {
    ...mapState(["isCollapse","menu"]),
  },
  methods: {
    //左侧栏放大缩小
    ...mapMutations(['setBarCollapse']),
    //显示、隐藏退出按钮
    showSetting() {
      this.login_flag = !this.login_flag
    },
    getUserInfo() { //获取用户信息
      let userName = this.$cookies.get("cname")
      let userId = this.$cookies.get("cid")
      this.user.userName = userName
      this.user.userId = userId
    },
    index() {
      this.$router.push({path: '/index'})
    },
    exit() {
      let role = this.$cookies.get("role")
      this.$router.push({path:"/"}) //跳转到登录页面
      this.$cookies.remove("cname") //清除cookie
      this.$cookies.remove("cid")
      this.$cookies.remove("role")
      if(role == 0) {
        this.menu.pop()
      }
    }
  },
  store
}
</script>

<style scoped>
#topbar {
  position: relative;
  z-index: 10;
  background-color: #409EFF;
  height: 80px;
  line-height: 80px;
  color: #fff;
  box-shadow: 5px 0px 10px rgba(0, 0, 0, 0.5);
  padding-right: 20px;
}
#topbar .topbar-left {
  height: 80px;
  display: flex;
  justify-content: center;
  background: rgba(0, 0, 0, 0.05);
  overflow: hidden;
}
.topbar-left .icon-kaoshi {
  font-size: 60px;
}
.topbar-left .title {
  font-size: 20px;
  cursor: pointer;
}
.topbar-right {
  padding-top: 5px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.topbar-right .user-img {
  width: 50px;
  height: 50px;
  border-radius: 50%;
}
.topbar-right .el-icon-menu {
  font-size: 30px;
  margin-left: 20px;
}
.topbar-right .user {
  position: relative;
  display: flex;
}
</style>
