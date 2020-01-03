<template>
  <el-card class="dashboard-container" style="margin-right:100px">
    <div class="app-container">
    <el-button size="small" @click="dialogVisible=true">新增技巧</el-button>
       <el-dialog :visible.sync="dialogVisible" :show-close="false" width="60%">
          <el-card>
              <el-form label-width="80px" :model="editForm" :rules="rules" ref="myForm">
                  <el-form-item label="标题" prop="title">
                    <el-input v-model="editForm.title"></el-input>
                  </el-form-item>
                  <el-form-item prop="articleBody">
                     <quill-editor style="height:200px;margin-bottom:90px"  v-model="editForm.articleBody">
                     </quill-editor>
                  </el-form-item>
                  <el-form-item label="视频地址">
                    <el-input  v-model="editForm.videoURL"></el-input>
                  </el-form-item>
              </el-form>
          </el-card>
         <span slot="footer" class="dialog-footer">
           <el-button @click="resetForm">取 消</el-button>
           <el-button type="primary" @click="submitForm">确 定</el-button>
         </span>
       </el-dialog>
    </div>
    <el-form inline>
      <el-form-item label="关键字 :">
        <el-input placeholder="请输入题目编号/题干" v-model="searchForm.keyword"></el-input>
      </el-form-item>
      <el-form-item>
            <el-button size="small" plain @click="clear">清除</el-button>
            <el-button size="small" type="primary" @click="search">搜索</el-button>
      </el-form-item>
    </el-form>
    <el-table :data="list" style="width: 100%;margin-left:20px" border>
      <el-table-column prop="id" label="序号" width="120"></el-table-column>
      <el-table-column prop="title" label="标题" width="500"></el-table-column>
      <el-table-column prop="visits" label="阅读人数"  width="120"></el-table-column>
      <el-table-column prop="state" label="状态"  width="120">
        <template slot-scope="obj">{{obj.row.state===1 ? "启用" : "禁用"}}</template>
      </el-table-column>
      <el-table-column prop="creator" label="录入人"  width="120"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="obj">
          <el-button size="small" type="text">预览</el-button>
          <el-button size="small" type="text" @click="delItem(obj.row.id)">删除</el-button>
          <el-button size="small" type="text">修改</el-button>
          <el-button size="small" type="text" @click="changeState(obj.row)">{{obj.row.state===1 ? "禁用" : "启用"}}</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-row type="flex" justify="center" style="height:60px" align="middle">
        <div class="block">
             <el-pagination
                 :current-page="page.currentPage"
                 :page-sizes="[10, 20, 30, 40]"
                 :page-size="page.pageSize"
                 layout="prev, pager, next, sizes, jumper"
                 :total="page.total">
             </el-pagination>
         </div>
    </el-row>
  </el-card>
</template>

<script>
import { list ,
         state as changeState,
         remove,
         add as editAdd} from "../../api/hmmm/articles"

export default {
  name: 'ArticlesList',
  data() {
    return {
      editForm:{
        title:'', //标题
        articleBody:'',//正文
        videoURL:'' //视频地址
      },
      rules:{
        title:[{ required: true, message: '请输入标题名称', trigger: 'blur' }],
        articleBody:[{ required: true, message: '请输入正文内容', trigger: 'blur' }],
        videoURL:[{ required: true, message: '请输入视频地址', trigger: 'blur' }],
      },
      list:[],
      page: {
        total: 0, // 总页数
        pageSize: 10, // 每页条数
        currentPage: 1 // 当前页数
      },
      //搜索对象
      searchForm:{
         keyword:''
      },
      //弹层
      dialogVisible: false //默认关闭
    }
  },
  methods:{
    //取消重置
    resetForm(){
      this.dialogVisible=false
      this.$refs.myForm.resetFields()
    },
    //手动校验
     async submitForm(){
       await this.$refs.myForm.validate();
      //  await editAdd();
      //   this.$message({ type: "success", message: "发布成功" });
      //   this.getlist(this.editForm);
      //   this.editForm={
      //     title:'', //标题
      //     articleBody:'',//正文
      //     videoURL:'' //视频地址
      //   };
      //   this.dialogVisible = false    
    },
    //搜索
    search(){
      this.page.currentPage=1
      this.getlist(this.searchForm)
    },
    //清除
    clear(){
      this.searchForm={
        input:''
      }
    },
    //删除
    async delItem(id){
      await this.$confirm('确定要删除吗?')
      await remove({id})
      this.$message({ type: "success", message: "删除成功" })
      this.getlist(this.searchForm); // 应该带状态查询
    },
    //改变状态方式
     async changeState(row){
       await this.$confirm('确定要改变状态吗?')
       await changeState({id: row.id, state: row.state===1 ? "0" : "1"})
       this.$message({ type: "success", message: "改变状态成功" })
       this.getlist(this.searchForm); // 应该带状态查询
      },
    //获取文章列表
    async getlist(data){
    let result= await list({
      page:this.page.currentPage,
      pages:this.page.pageSize,
      ...data
    })
      this.list=result.data.items;
      this.page.total=result.data.counts
    },
  },
  created(){
    this.getlist()
  }
}
</script>

<style scoped>
  
</style>
