<template>
      <el-card>
          <!-- 新增学科 -->
        <el-row slot="header">
          <el-button type="primary" @click="subjectVisible">新增学科</el-button>
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
                <el-button @click="toDirectorys" size="small" type="text">学科分类</el-button>
                <el-button @click="toTags" size="small" type="text">学科标签</el-button>
                <el-button @click="changeForm(obj.row.id)" size="small" type="text">修改</el-button>
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

        <!-- 新增和修改信息弹窗 -->
        <el-dialog :show-close="false" :visible="dialogVisible">
          <el-form :model="formData"  label-width="150px" ref="myForm" :rules="rules">
            <!-- 新增或修改内容 -->
            <el-form-item label="学科名称" prop="subjectName">
              <el-input v-model="formData.subjectName"></el-input>
            </el-form-item>
            <!-- 显示开关 -->
            <el-form-item label="是否前台显示" prop="isFrontDisplay">
              <el-switch v-model="formData.isFrontDisplay">

              </el-switch>              
            </el-form-item>
          </el-form>
          <el-row slot="footer" type="flex" justify="end">
            <el-button type="primary" @click="addForm">确定</el-button>
            <el-button @click="btnCancel">取消</el-button>
          </el-row>
        </el-dialog>

      </el-card>
</template>

<script>
import { list , remove , add , detail , update } from '../../api/hmmm/subjects'
export default {
  // name: 'SubjectsList',
  data() {
    return {
      formData:{ subjectName:"" ,isFrontDisplay:true}, // 添加条件
      rules:{subjectName:[{required: true, message: "学科名称不能为空", trigger: "blur"}]},
      dialogVisible:false, //控制弹窗显示
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
    //跳转到标签页面
    toTags(){
      this.$router.push('tags')
    },
    //跳转到标签页面
    toDirectorys(){
      this.$router.push('directorys')
    },
    //点击修改
    async changeForm(id){
      let result = await detail({id});
      console.log(result.data);
      this.formData=result.data
      this.formData.isFrontDisplay===1 ? this.formData.isFrontDisplay=true : this.formData.isFrontDisplay=false
      this.dialogVisible=true
    },
    //点击显示弹窗
    subjectVisible(){
      this.dialogVisible=true
    },
    // 点击确定增加
    async addForm(){
      await this.$refs.myForm.validate();
      this.formData.id ? await update(this.formData) : await  add(this.formData)
      this.formData={ 
        subjectName:"" ,
        isFrontDisplay:true
        }
      this.dialogVisible=false;
      this.$message({ type: "success", message: "添加成功" });
      this.getSubjects(this.searchForm)
    },
    // 点击取消
    btnCancel(){
      this.formData={ 
        subjectName:"" ,
        isFrontDisplay:true
        }
      this.dialogVisible=false;
    },
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
