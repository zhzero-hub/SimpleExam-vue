<!-- 用户登录界面 -->
<template>
  <div id="login">
    <div class="bg"></div>
    <el-row class="main-container">
      <el-col :lg="8" :xs="16" :md="10" :span="10">
        <div class="top"
             style="text-align: center;
                    margin-top: 100px;
                    color: #409EFF;">
          <i class="iconfont icon-kaoshi"></i>
        </div>
        <div style="font-size: 30px;
                    font-weight: 500!important;
                    text-align: center;
                    font-family: inherit;
                    color: #191d21!important;">
          在线考试系统
        </div>
        <div style="text-align: center;
                    font-family: nunito,segoe ui,arial,serif;
                    font-size: 16px;
                    font-weight: 500;
                    color: #98a6ad!important">
          Welcome to the online exam system
        </div>

          <div class="container" style="width: 70%; position: center">
            <el-form :label-position="labelPosition" label-width="80px" :model="formLabelAlign">
              <el-form-item label="用户名" id="form-label1" class="form-label"></el-form-item>
              <el-input v-model.number="formLabelAlign.username" placeholder="请输入用户名" class="form-content"></el-input>
              <el-form-item label="密码" id="form-label2" class="form-label"></el-form-item>
              <el-input v-model="formLabelAlign.password" placeholder="请输入密码" type='password' class="form-content"></el-input>

              <el-checkbox v-model="checked" class="remindMe">记住我</el-checkbox>

              <div class="submit">
                <el-button type="primary" class="row-login" @click="login()" :loading="isLoading">登录</el-button>
              </div>
              <!-- <div class="options">
                <p class="find"><a href="javascript:;">找回密码</a></p>
                <div class="register">
                  <span>没有账号?</span>
                  <span><a href="javascript:;">去注册</a></span>
                </div>
              </div> -->
            </el-form>
          </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import {mapState} from 'vuex'
import store from '@/store/store'
export default {
  store,
  name: "login",
  data() {
    return {
      checked: false,
      isLoading: false,
      role: 2,
      labelPosition: 'left',
      formLabelAlign: {
        username: localStorage.getItem("rem_userName"),
        password: localStorage.getItem("rem_userPwd")
      }
    }
  },
  methods: {
    //用户登录请求后台处理
    login() {
      console.log("登录操作执行-------");
      this.isLoading = true
      if(this.checked) {
        localStorage.setItem("rem_userName", this.formLabelAlign.username);
        localStorage.setItem("rem_userPwd", this.formLabelAlign.password);
      }
      else {
        localStorage.removeItem("rem_userName");
        localStorage.removeItem("rem_userPwd");
      }
      this.$axios({
        url: `/api/login`,
        method: 'post',
        data: {
          ...this.formLabelAlign
        }
      }).then(res=>{
        let resData = res.data.data
        if(resData != null) {
          switch(resData.role) {
            case "0":  //管理员
              this.$cookies.set("cname", resData.adminName)
              this.$cookies.set("cid", resData.adminId)
              this.$cookies.set("role", 0)
              this.$router.push({path: '/index' }) //跳转到首页
              break
            case "1": //教师
              this.$cookies.set("cname", resData.teacherName)
              this.$cookies.set("cid", resData.teacherId)
              this.$cookies.set("role", 1)
              this.$router.push({path: '/index' }) //跳转到教师用户
              break
            case "2": //学生
              this.$cookies.set("cname", resData.userId)
              this.$cookies.set("cid", resData.userId)
              this.$router.push({path: '/student'})
              break
          }
          this.isLoading = false
        }
        else { //错误提示
          this.$message({
            showClose: true,
            type: 'error',
            message: '用户名或者密码错误'
          })
          this.isLoading = false
        }
      })
    },
    clickTag(key) {
      this.role = key
    }
  },
  computed: {
    userInfo() {
      return this.$store.state.userInfo
    },
  },
  mounted() {

  }
}
</script>

<style lang="scss" scoped>
.remind {
  border-radius: 4px;
  padding: 10px 20px;
  display: flex;
  position: fixed;
  right: 20px;
  bottom: 50%;
  flex-direction: column;
  color: #606266;
  background-color: #fff;
  border-left: 4px solid #409eff;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19)
}
.container {
  line-height: 1.5;
  margin: 0 auto;
}
.container .el-radio-group {
  margin: 30px 0px;
}
a:link {
  color:#ff962a;
  text-decoration:none;
}
#login {
  font-size: 14px;
  color: #000;
  background-color: #fff;
}
#login .bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  overflow-y: auto;
  height: 100%;
}
#login .main-container {
  display: flex;
  justify-content: center;
  align-items: center;
  line-height: 300%;
}
#login .top .icon-kaoshi {
  font-size: 80px;
}
.simple-exam {
  align-content: center;
  font-size: 30px;
  margin-top: 20px;
}
#login .bottom {
  display:flex;
  justify-content: center;
  background-color:#fff;
  border-radius: 5px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
#login .bottom .title {
  text-align: center;
  font-size: 30px;
}
.bottom .container .title {
  margin: 30px 0px;;
}

.submit {
  line-height: 50px;
  text-align: right;
}
.row-login {
  padding: .95rem 1.5rem;
  font-size: 12px;
  color: #000000;
  font-weight: 600;
  box-shadow: 0 2px 6px #acb5f6;
  background-color: #6777ef;
  border-color: #6777ef;
}
.row-login:hover {
  color: #fff;
  background-color: #394eea;
}
.remindMe {
  line-height: 100px;
}
.remindMe >>> .el-checkbox__input.is-checked .el-checkbox__inner, .el-checkbox__input.is-indeterminate .el-checkbox__inner{
  background-color: #6777ef;
  font-weight: bold;
  border: 1px solid #bfdeff;
}
.remindMe >>> .el-checkbox__input.is-checked + .el-checkbox__label {
  color: #000000;
}
.remindMe >>> .el-checkbox.is-bordered.is-checked{
  border: 1px solid #bfdeff;
}
.remindMe >>> .el-checkbox__input.is-focus .el-checkbox__inner{
  border: 1px solid #bfdeff;
  font-weight: bold;
}

#form-label1 >>> .el-form-item__label {
  line-height: 0;
  font-weight: 600;
  color: #34395e;
  font-size: 10px;
  padding-top: 50px;
}
#form-label2 >>> .el-form-item__label {
  line-height: 0;
  font-weight: 600;
  color: #34395e;
  font-size: 10px;
  padding-top: 30px;
}
.form-label {
  font-weight: 600;
  color: #34395e;
  font-size: 10px;
  padding-top: 10px;
}

.form-content >>> .el-input__inner {
  background-color: #fdfdff;
  border-color: #e4e6fc;
}
.form-content >>> .el-input__inner:focus{
  border: 1px solid #4D7CFE;
}

</style>
