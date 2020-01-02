<template>
  <el-table
    :data="list"
    border
    style="width: 100%">
    <el-table-column
      prop="id"
      label="序号"
      width="180">
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
      prop=" progressOfAnswer"
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
      label="操作">
    </el-table-column>
  </el-table>
</template>

<script>
import {randoms} from '../../api/hmmm/questions.js'
 export default {
      data() {
        return {
          list:[]//等一个个数据接收返回结果
        }
      },
      methods:{
         aA(){
            randoms().then(res=>{
              this.list=res.data.items
                 console.log(res)
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
