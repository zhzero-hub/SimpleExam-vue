<template>
  <div id="nav">
    <el-page-header @back="goBack" :content=content.item></el-page-header>
  </div>
</template>

<script>
import store from '@/store/store'
import {mapState} from 'vuex'

export default {
  data() {
    return {
      active: [],
      content: [],
      index1: null,
    }
  },
  computed: {
    ...mapState(["menu" , "adminMenu"])
  },
  methods: {
    getIndex() {
      this.bus.$on('sendIndex',(data)=>{
        if(data > this.menu.length){
          data = data - this.menu.length
          this.active = this.adminMenu[data - 1]
        }
        else {
          this.active = this.menu[data - 1]
        }
        this.index1 = data
      })
      this.bus.$on('itemIndex' , (data) => {
        this.content = this.active.content[data]
      })
    },
    goBack() {
      this.$router.go(-1)
    }
  },
  created() {
    this.getIndex()
  }
}
</script>

<style scoped>
#nav .el-page-header {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  line-height: 50px;
  padding-left: 20px;
}
#nav .el-page-header__content {
  font-size: 22px;
  color: #303133;
  padding-left: 5px;
}
</style>
