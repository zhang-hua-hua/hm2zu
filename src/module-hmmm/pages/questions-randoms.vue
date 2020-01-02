<template>
  <div class="dashboard-container">
    
   <el-table
      :data="list"
      border
     
      style="width: 100%">
      <el-table-column
        prop="numberid"
        label="序号"
        width="60"
        >
      </el-table-column>
      <el-table-column
        prop="addTime"
        label="组题时间"
        >
      </el-table-column>
      <el-table-column
        prop="userName"
        label="用户名"
        width="80"
        >
      </el-table-column>
      <el-table-column
        prop="questionIDs"
        label="试题ID"
        >
      </el-table-column>
      <el-table-column
        prop="progressOfAnswer"
        label="答题进度"
        >
      </el-table-column>
       <el-table-column
        prop="accuracyRate"
        label="正确率"
        >
      </el-table-column>
       <el-table-column
        prop="totalSeconds"
        label="答题总耗时（秒）"
        >
      </el-table-column>
       <el-table-column
        prop="questionType"
        label="组题类型/详情"
        >
      </el-table-column>
      <el-table-column
        label="操作">
        <template slot-scope="obj">
          <el-button @click="delQuestions({'id':obj.row.id})" size='small' type="text">删除</el-button>
        </template>
        
      </el-table-column>
    </el-table>
    <el-row type='flex' justify='center' align="middle" style="height:80px">
      <span>共{{toatals}}条</span>
      <el-pagination
        :page-size='page.pageSize'
        :current-page='page.currentPage'
        background
        layout="prev, pager, next"
        @current-change='pagechange'
        :total="page.total">
      </el-pagination>
    </el-row>
  </div>
</template>

<script>
import {randoms,removeRandoms} from '../../api/hmmm/questions'

export default {
  name: 'QuestionsRandoms',
  data() {
    return {
      // numberid:-1,
      toatals:'',
      list:[],
      result:{},
       page: {
        total: 0,
        pageSize: 10,
        currentPage: 1
      }
    }
  },
  methods:{ 
    pagechange (newpage) {
      console.log(newpage);
      this.page.currentPage = newpage
      this.getListOfGroupQuestions()
    },
    delQuestions(id){
      console.log(id)
      this.$confirm('确定删除数据').then(res=>{
        removeRandoms(id).then(res=>{
        this.$message({ type: "success", message: "删除成功" });
         this.getListOfGroupQuestions()
      })
      })
      
    },
    getListOfGroupQuestions(){
      // console.log(11)
      randoms({page:this.page.currentPage,pagesize:this.page.pageSize}).then(res=>{
        console.log(res);
        this.page.total=res.data.counts
        this.toatals=res.data.counts
        this.list=res.data.items.map((item,index)=>{
          // this.numberid=item.id
          item.numberid=index+1
          // item.id=index+1
          item.progressOfAnswer=item.progressOfAnswer+'%'
          item.accuracyRate= item.accuracyRate+'%'
          item.questionIDs= item.questionIDs[0]
         return item
        })
        // console.log(this.list);
        // console.log(this.page.total);
        //  console.log(this.page);
      })
    }
  },
  created(){
    this.getListOfGroupQuestions()
  }
}
</script>

<style scoped>
</style>
