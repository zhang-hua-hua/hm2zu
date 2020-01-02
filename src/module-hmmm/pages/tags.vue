<template>
  <div class="dashboard-container">
    <el-card>
      <div slot="header">
        <el-button type='primary'>新增标签</el-button>
        <el-button type='primary'>返回学科</el-button>
      </div>
      <el-form label-width='100px'>
        <el-form-item label='标签名称:'>
          <el-input style="width:220px;margin-right:10px;"></el-input>
          <el-button>清除</el-button>
          <el-button type='primary'>搜索</el-button>
        </el-form-item>
      </el-form>
      <el-table>
        <el-table-column label='序号' prop=''></el-table-column>
        <el-table-column label='标签名称' prop=''></el-table-column>
        <el-table-column label='创建者' prop=''></el-table-column>
        <el-table-column label='面试题数量' prop=''></el-table-column>
        <el-table-column label='状态' prop=''></el-table-column>
        <el-table-column label='操作'>
          <el-button type="text" size="small">修改</el-button>
          <el-button type="text" size="small">禁用</el-button>
          <el-button type="text" size="small">删除</el-button>
        </el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script>
export default {
  name: 'TagsList',
  data() {
    return {}
  }
}
</script>

<style scoped>
</style>
