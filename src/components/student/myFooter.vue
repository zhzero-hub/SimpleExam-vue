<!--页脚部分-->
<template>
  <footer id="footer">
    <ul>
      <li>南京航空航天大学 计算机科学与技术学院/人工智能学院 软件工程课程设计 2020年秋</li>
      <li>161820217 郑好</li>
    </ul>
  </footer>
</template>

<script>
export default {
  name: "myFooter"
}
</script>

<style scoped>
#footer a {
  color: #919698;
  font-size: 14px;
}
#footer {
  background-color: #eee;
}
#footer ul {
  margin-top: 40px;
  border-top: 1px solid #d5d5d5;
  display: flex;
  justify-content: center;
  height: 80px;
  line-height: 80px;
}
#footer ul li {
  color: #919698;
  font-size: 14px;
  margin-right: 20px;
}
</style>
