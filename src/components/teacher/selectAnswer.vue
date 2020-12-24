<template>
  <div class="exam">
    <el-table :data="pagination.records" :row-class-name="tableRowClassName"
              @row-click="handleEdit" highlight-current-row stripe border>
      <el-table-column fixed="left" prop="subject" label="试卷名称" width="180" align="center"></el-table-column>
      <el-table-column prop="questionId" label="题目编号" width="100" align="center"></el-table-column>
      <el-table-column prop="question" label="题目信息" width="500" align="center"></el-table-column>
      <el-table-column prop="section" label="所属章节" width="200" align="center"></el-table-column>
      <el-table-column prop="type" label="题目类型" width="200" align="center"></el-table-column>
      <el-table-column prop="score" label="试题分数" width="150" align="center"></el-table-column>
      <el-table-column prop="level" label="难度等级" width="150" align="center"></el-table-column>
      <el-table-column label="查看详情" width="300" align="center">
        <template slot-scope="scope">
          <el-button @click="checkQuestion(scope.row.questionId)" type="primary" size="small">查看详情</el-button>
          <el-button @click="editQuestion(scope.row.questionId)" type="primary" size="small">编辑</el-button>
          <el-button @click="deleteRecord(scope.row.questionId)" type="danger" size="small">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pagination.current"
      :page-sizes="[6, 10]"
      :page-size="pagination.size"
      layout="total, sizes, prev, pager, next, jumper"
      :total="pagination.total"
      class="page">
    </el-pagination>
    <!-- 编辑对话框-->
    <el-dialog
      title="题库信息"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose">
      <section class="update">
        <el-form ref="form" :model="form" label-width="80px">
          <template v-if="readonly">
            <el-form-item label="试卷名称">
              <el-input v-model="form.subject" readonly></el-input>
            </el-form-item>
            <el-form-item label="题目信息">
              <el-input v-model="form.question" readonly></el-input>
            </el-form-item>
            <el-form-item label="选项A">
              <el-input v-model="form.answerA" readonly></el-input>
            </el-form-item>
            <el-form-item label="选项B">
              <el-input v-model="form.answerB" readonly></el-input>
            </el-form-item>
            <el-form-item label="选项C">
              <el-input v-model="form.answerC" readonly></el-input>
            </el-form-item>
            <el-form-item label="选项D">
              <el-input v-model="form.answerD" readonly></el-input>
            </el-form-item>
            <el-form-item label="答案">
              <el-input v-model="form.rightAnswer" readonly></el-input>
            </el-form-item>
            <el-form-item label="所属章节">
              <el-input v-model="form.section" readonly></el-input>
            </el-form-item>
            <el-form-item label="题目类型">
              <el-input v-model="form.type" readonly></el-input>
            </el-form-item>
            <el-form-item label="试题分数">
              <el-input v-model="form.score" readonly></el-input>
            </el-form-item>
            <el-form-item label="难度等级">
              <el-input v-model="form.level" readonly></el-input>
            </el-form-item>
          </template>
          <template v-else>
            <el-form-item label="试卷名称">
              <el-input v-model="form.subject"></el-input>
            </el-form-item>
            <el-form-item label="题目信息">
              <el-input v-model="form.question"></el-input>
            </el-form-item>
            <el-form-item label="选项A">
              <el-input v-model="form.answerA"></el-input>
            </el-form-item>
            <el-form-item label="选项B">
              <el-input v-model="form.answerB"></el-input>
            </el-form-item>
            <el-form-item label="选项C">
              <el-input v-model="form.answerC"></el-input>
            </el-form-item>
            <el-form-item label="选项D">
              <el-input v-model="form.answerD"></el-input>
            </el-form-item>
            <el-form-item label="答案">
              <el-input v-model="form.rightAnswer"></el-input>
            </el-form-item>
            <el-form-item label="所属章节">
              <el-input v-model="form.section"></el-input>
            </el-form-item>
            <el-form-item label="题目类型">
              <el-input v-model="form.type"></el-input>
            </el-form-item>
            <el-form-item label="试题分数">
              <el-input v-model="form.score"></el-input>
            </el-form-item>
            <el-form-item label="难度等级">
              <el-input v-model="form.level"></el-input>
            </el-form-item>
          </template>
        </el-form>
      </section>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="submit()">确 定</el-button>
      </span>
    </el-dialog>

  </div>
</template>

<script>
export default {
  data() {
    return {
      pagination: {
        //分页后的考试信息
        current: 1, //当前页
        total: null, //记录条数
        size: 6 //每页条数
      },
      dialogVisible: false,
      form: {},
      readonly: false
    };
  },
  created() {
    this.getAnswerInfo();
  },
  methods: {
    getAnswerInfo() {
      //分页查询所有试卷信息
      this.$axios(
        `/api/answers/multi/${this.pagination.current}/${this.pagination.size}`
      ).then(res => {
          this.pagination = res.data.data;
          console.log(res);
        }).catch(error => {});
    },
    checkQuestion(questionId) {
      this.readonly = true
      this.dialogVisible = true
      this.$axios(`/api/answer/multi/${questionId}`).then(res => {
        this.form = res.data.data
      })
    },
    editQuestion(questionId) { //修改学生信息
      this.readonly = false
      this.dialogVisible = true
      this.$axios(`/api/answer/multi/${questionId}`).then(res => {
        this.form = res.data.data
      })
    },
    handleEdit() {

    },
    handleClose(done) { //关闭提醒
      this.$confirm('确认关闭？')
        .then(_ => {
          done();
        }).catch(_ => {});
    },
    submit() {
      this.dialogVisible = false
      this.$axios({
        url: '/api/answers/multi',
        method: 'put',
        data: {
          ...this.form
        }
      }).then(res => {
        console.log(res)
        if(res.data.code === 200) {
          this.$message({
            message: '更新成功',
            type: 'success'
          })
        }
        this.getAnswerInfo()
      })
    },
    deleteRecord(answerCode) {
        this.$confirm("确定删除该记录吗,该操作不可逆！！！","删除提示",{
        confirmButtonText: '确定删除',
        cancelButtonText: '取消',
        type: 'danger'
      }).then(()=> { //确认删除
        this.$axios({
          url: `/api/answer/multi/${answerCode}`,
          method: 'delete',
        }).then(res => {
          this.getAnswerInfo()
        })
      }).catch(() => {

      })
    },
    //改变当前记录条数
    handleSizeChange(val) {
      this.pagination.size = val;
      this.getAnswerInfo();
    },
    //改变当前页码，重新发送请求
    handleCurrentChange(val) {
      this.pagination.current = val;
      this.getAnswerInfo();
    },
    tableRowClassName({ row, rowIndex }) {
      if (rowIndex % 2 === 0) {
        return "warning-row";
      } else {
        return "success-row";
      }
    }
  }
};
</script>
<style lang="scss" scoped>
.exam {
  padding: 0px 40px;
  .page {
    margin-top: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .edit {
    margin-left: 20px;
  }
  .el-table tr {
    background-color: #DD5862 !important;
  }
}
  .el-table .warning-row {
    background: #000 !important;

  }

  .el-table .success-row {
    background: #DD5862;
  }

</style>
