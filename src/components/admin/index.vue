<template>
  <el-container>
    <el-header>
      <header1></header1>
    </el-header>
    <el-container>
      <el-aside width="auto">
        <main-left></main-left>
      </el-aside>
      <el-container>
        <el-main>
          <navigator style="padding-top: 10px"></navigator>
          <router-view style="padding-top: 20px"></router-view>
        </el-main>
      </el-container>
    </el-container>
  </el-container>
</template>

<script>
import header from '@/components/common/header'
import mainLeft from '@/components/common/mainLeft'
import navigator from '@/components/common/navigator'
export default {
  components:{
    header1: header,
    mainLeft: mainLeft,
    navigator:navigator
  },
  data() {
    return {
      username: ''
    }
  },
  methods: {

  }
}
</script>

<style lang="scss" scoped>
#index .nav {
  box-shadow: 1px 0 5px rgba(0, 0, 0, 0.1);
  margin-bottom: 30px;
}
.container {
  display: flex;
  background-color: #fff;
}
.main_wrapper {
  overflow: hidden;
  flex: 1;
  background-color: #fff;
}
.el-header {
  padding-left: 0;
  padding-right: 0;
  text-align: right;
}

body > .el-container {
  margin-bottom: 40px;
}


</style>

