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
      <el-table :data='dataList'>
        <el-table-column label='序号' prop='id'></el-table-column>
        <el-table-column label='标签名称' prop='tagName'></el-table-column>
        <el-table-column label='创建者' prop='creator'></el-table-column>
        <el-table-column label='面试题数量' prop='totals'></el-table-column>
        <!-- table-column标签上有一个formattershuxing  用来格式化内容 -->
        <el-table-column :formatter='formatterState' label='状态' prop='state'></el-table-column>
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
import { list } from '../../api/hmmm/tags'
export default {
  name: 'TagsList',
  data() {
    return {
      dataList: [] //设置变量接收返回标签数据
    }
  },
  methods: {
    // 根据标签状态显示 内容
    formatterState (row,column,cellValue,index) {
      // row 代表当前行数据
      // column 当前列信息
      // cellValue 当前的单元格的值
      // index 索引
      return this.dataList.state ? '屏蔽':'开启'
    },
    // 获取标签数据
    getDataList () {
      list().then(res => {
        // console.log(res)
        this.dataList = res.data.items
      })
    }
  },
  created () {
    this.getDataList()
  }
}
</script>

<style scoped>
</style>
