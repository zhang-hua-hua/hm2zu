<template>
  <div class="dashboard-container">
    
   <el-table
      :data="list"
      border
      style="width: 100%">
      <el-table-column
        :prop="id"
        label="序号"
        width="60px"
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
        prop="address"
        label="操作">
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'QuestionsRandoms',
  data() {
    return {
      list:[]
    }
  }
}
</script>

<style scoped>
</style>
