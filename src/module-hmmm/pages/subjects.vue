<template>
      <el-card>
          <!-- 面包屑 -->
        <el-row slot="header">
          <el-button type="primary">新增学科</el-button>
        </el-row>

        <!-- 搜索表单 -->
        <el-form inline>
          <el-form-item label="学科名称">
            <el-input v-model="searchForm.subjectName"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button @click="clear">清除</el-button>
            <el-button @click="search" type="primary">搜索</el-button>
          </el-form-item>
        </el-form>

        <!-- 显示表格 -->
        <el-row>
          <el-table :data="list">
            <el-table-column prop="id" label="序号" align="center"></el-table-column>

            <el-table-column prop="subjectName" label="学科名称" align="center"></el-table-column>

            <el-table-column label="创建者"></el-table-column>

            <el-table-column prop="addDate" label="创建日期" width="180" align="center">
              <template slot-scope="obj">
                {{obj.row.addDate | parseTimeByString}}
              </template>
            </el-table-column>

            <el-table-column prop="isFrontDisplay" label="前台是否显示" align="center">
              <template slot-scope="obj">
                {{ obj.row.isFrontDisplay===1 ? '显示' :'-' }}
              </template>
            </el-table-column>

            <el-table-column prop="tags" label="二级目录" align="center"></el-table-column>

            <el-table-column prop="totals" label="标签" align="center"></el-table-column>

            <el-table-column prop="twoLevelDirectory" label="题目数量" align="center"></el-table-column>

            <el-table-column label="操作" width="220" align="center">
              <template slot-scope="obj">
                <el-button size="small" type="text">学科分类</el-button>
                <el-button size="small" type="text">学科标签</el-button>
                <el-button size="small" type="text">修改</el-button>
                <el-button @click="delItem(obj.row.id)" size="small" type="text">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
        </el-row>
        
        <!-- 分页 -->
        <el-row style="height:80px;" type="flex" justify="end" align="middle">
          <el-pagination
            background
            layout="total,prev, pager, next"
            :page-size="page.pageSize"
            :current-page="page.currentPage"
            :total="page.total"
            @current-change="changePage">
          </el-pagination>
        </el-row>
      </el-card>
</template>

<script>
import { list , remove } from '../../api/hmmm/subjects'
export default {
  // name: 'SubjectsList',
  data() {
    return {
      searchForm:{ subjectName:"" }, // 搜索条件
      list:[], //接受详细数据
      page:{
        currentPage:1, //当前页数
        pageSize:10, //每页显示个数
        total:1 //总条目数
      } //分页信息
    }
  },
  methods:{
    //点击删除
    async delItem(id){
      await this.$confirm("您是否确定删除此数据?");
      //确定删除
      await remove({ id });
      this.$message({ type: "success", message: "删除成功" });
      this.getSubjects(this.searchForm)
    },
    //搜索按钮
    search(){
      this.page.currentPage = 1
      this.getSubjects(this.searchForm)
    },
    //清除按钮
    clear(){
      this.searchForm.subjectName=''
    },
    //点击分页
    changePage(newPage){
      this.page.currentPage=newPage;
      this.getSubjects()
    },
    //获取列表数据
   async getSubjects(data){
     let result= await list({
       page: this.page.currentPage,
       pagesize: this.page.pageSize,
       ...data
     });
     this.list=result.data.items; // 列表数据
     this.page.total=result.data.counts; // 总条数
   }
  },
  created(){
    this.getSubjects()
  }
}
</script>

<style scoped>

</style>
