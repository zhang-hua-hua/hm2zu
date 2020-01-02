<template>
  <el-card class="dashboard-container" style="margin-right:100px">
    <div class="app-container">
    <el-button size="small" @click="$router.push({name:'articles-add'})">新增技巧</el-button>
    </div>
    <el-row>
      <el-col :span="12">
        <el-input placeholder="请输入题目编号/题干" v-model="input" style="width:300px;margin-left:20px;margin-bottom:10px">
          <template slot="prepend">关键字</template>
        </el-input>
      </el-col>
      <el-col :span="12">
        <el-row type="flex" justify="end" style="margin-right:30px">
            <el-button size="small" plain >清除</el-button>
            <el-button size="small" type="primary">搜索</el-button>
        </el-row>
      </el-col>
    </el-row>
    <el-table :data="list" style="width: 100%;margin-left:20px" border>
      <el-table-column prop="id" label="序号" width="120"></el-table-column>
      <el-table-column prop="title" label="标题" width="500"></el-table-column>
      <el-table-column prop="reads" label="阅读人数"  width="120"></el-table-column>
      <el-table-column :formatter="openOrDown" prop="state" label="状态"  width="120"></el-table-column>
      <el-table-column prop="creator" label="录入人"  width="120"></el-table-column>
      <el-table-column label="操作">
        <el-button size="small" type="text">预览</el-button>
        <el-button size="small" type="text">删除</el-button>
        <el-button size="small" type="text">修改</el-button>
        <el-button size="small" type="text">禁用</el-button>
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
import { list } from "../../api/hmmm/articles"
import { log } from 'util'
export default {
  name: 'ArticlesList',
  data() {
    return {
      input:'',
      list:[],
      page: {
        total: 0, // 总页数
        pageSize: 10, // 每页条数
        currentPage: 1 // 当前页数
      }
    }
  },
  methods:{
    //状态方式
    openOrDown(row, column, cellValue, index){
      return cellValue ? "启用" : "禁用"
    },
    //获取文章列表
    getlist(){
      list().then(result=>{
        this.list=result.data.items;
      })
    },
  },
  created(){
    this.getlist()
  }
}
</script>

<style scoped>
  
</style>
