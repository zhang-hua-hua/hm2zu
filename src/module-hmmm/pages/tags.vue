<template>
  <div class="dashboard-container">
    <el-card>
      <div slot="header">
        <el-button type='primary'>新增标签</el-button>
        <el-button type='primary'>返回学科</el-button>
      </div>
      <el-form label-width='100px'>
        <el-form-item label='标签名称:'>
          <el-input v-model="searchForm.tagName" style="width:220px;margin-right:10px;"></el-input>
          <el-button @click='searchForm.tagName = ""'>清除</el-button>
          <el-button @click='search()' type='primary'>搜索</el-button>
        </el-form-item>
      </el-form>
      <el-table :data='dataList'>
        <el-table-column label='序号' prop='id'></el-table-column>
        <el-table-column label='标签名称' prop='tagName'></el-table-column>
        <el-table-column label='创建者' prop='creator'></el-table-column>
        <el-table-column label='面试题数量' prop='totals'></el-table-column>
        <!-- table-column标签上有一个formatter属性  用来格式化内容；第二种方法可以用作用域插槽+过滤器-->
        <el-table-column :formatter='formatterState' label='状态' prop='state'></el-table-column>
        <el-table-column label='操作'>
          <template slot-scope="scope">
            <el-button type="text" size="small">修改</el-button>
            <el-button @click='change(scope.row)' type="text" size="small">{{scope.row.state === 1 ? '禁用' : '开启'}}</el-button>
            <el-button type="text" size="small">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-row type='flex' justify='end'>
          <el-pagination
          background
          layout="total, prev, pager, next"
          :total="pageObj.total"
          :current-page='pageObj.currentPage'
          :page-size='pageObj.pageSize'
          @current-change='changePage'>
        </el-pagination>
      </el-row>
    </el-card>
  </div>
</template>

<script>
import { list, removeState as changeState } from '../../api/hmmm/tags'
export default {
  name: 'TagsList',
  data() {
    return {
      dataList: [], //设置变量接收返回标签数据
      pageObj: {
        total: 0,
        currentPage: 1,
        pageSize: 10
      },
      searchForm: {
        tagName: ''
      }
    }
  },
  methods: {
    // 关闭或开启禁用
    async change (row) {
      await this.$confirm('确定改变状态吗?',{
        confirmButtonText:'确定',
        cancelButtonText:'取消',
        type: 'warning'
      });
      await changeState({id: row.id, state: row.state === 1 ? 0 : 1});
      this.$message({ type: 'success', message: '改变状态成功' });
      this.getDataList(this.searchForm)  //应该带状态查询
    },    
    // 搜索标签
    search () {
      // alert(searchTag)
      // 当输入为空时 搜索全部
      // if (searchTag) {
      //     this.dataList = this.dataList.filter(item => {
      //     return item.tagName === searchTag})
      // }else {
      //   this.getDataList()
      // }
      this.pageObj.currentPage = 1; //搜索时回到第一页 获取数据是也要带上搜素条件
        this.getDataList(this.searchForm)
    },
    // 改变当前页码
    changePage (newPage) {
      this.pageObj.currentPage = newPage
      this.getDataList(this.searchForm)
    },
    // 根据标签状态显示 内容
    formatterState (row,column,cellValue,index) {
      // row 代表当前行数据
      // column 当前列信息
      // cellValue 当前的单元格的值
      // index 索引
      return row.state === 1 ? '开启':'禁用'
    },
    // 获取标签数据
    getDataList (data) {
      list({
        page: this.pageObj.currentPage,
        pageSize: this.pageObj.pageSize,
        ...data
      }).then(res => {
        // console.log(res)
        this.dataList = res.data.items
        this.pageObj.total = res.data.counts
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
