<template>
<div>
  <el-table
    :data="list"
    border
    style="width: 100%">
    <el-table-column
      prop="id"
      label="序号"
      width="100">
    </el-table-column>
    <el-table-column
      prop="addTime"
      label="组题时间"
      width="180">
    </el-table-column>
    <el-table-column
      prop="userName"
      label="用户名">
    </el-table-column>
     <el-table-column
      prop="questionIDs"
      label="试题ID">
    </el-table-column>
     <el-table-column
      prop="progressOfAnswer"
      label=答题进度>
    </el-table-column>
     <el-table-column
      prop="accuracyRate"
      label="正确率">
    </el-table-column>
     <el-table-column
      prop="totalSeconds"
      label="答题总耗时（秒）">
    </el-table-column>
     <el-table-column
      prop="questionType"
      label="操作"
       width="120">
       <template slot-scope="obj">
        <el-button @click="remove({'id':obj.row.id})" type="text" size="small">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
  <!-- 分页 -->
  <el-row type='flex' justify="center" style="height:80px">
    <!-- total 总页码   :page-size每页多少条 -->
   <el-pagination background layout="prev, pager, next" 
   :total="page.total"
   :page-size="page.pageSize"
   :current-page="page.currentPage"
   @current-change="changePage"
   ></el-pagination>
  </el-row>
  </div>
</template>

<script>
import {randoms,remove} from '../../api/hmmm/questions.js'
 export default {
      data() {
        return {
          list:[],//等一个个数据接收返回结果
          page:{
            total:0,//总页数
            pageSize:10,//默认每页条
            currentPage:1 //默认页码为1
          }//专门存放分页信息数据
        }
      },
      methods:{
        // 删除方法
        remove(id){
          this.$confirm('是否要删除该文章').then(res=>{
            remove(id).then(res=>{
                this.$message({ message: '删除成功',type: 'success'});
            })
              
            // 调取删除接口
          })
        },
        //页码改变事件
        changePage(newPage){
          this.page.currentPage=newPage;
          this.aA()
        },
         aA(){
            randoms().then(res=>{   
              // this.list=res.data.items
                 console.log(res);
                 this.page.total=res.data.counts;
                 this.list=res.data.items.map((element,index) => {
                    element.id=index+1
                     return element
                 });
            })
         }
      },
      created(){    //钩子函数就是页面渲染时执行的
         this.aA();
      }
    }
</script>

<style scoped>
</style>
