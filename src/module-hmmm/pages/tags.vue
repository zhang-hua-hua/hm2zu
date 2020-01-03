<template>
    <el-card>
      <div slot="header">
        <el-button @click='dialogVisible = true' type='primary'>新增标签</el-button>
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
            <el-button @click='modify(scope.row.id)' type="text" size="small">修改</el-button>
            <el-button @click='change(scope.row)' type="text" size="small">{{scope.row.state === 1 ? '禁用' : '开启'}}</el-button>
            <el-button @click='deltag(scope.row.id)' type="text" size="small">删除</el-button>
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
      <el-dialog
        title="新建标签"
        :visible="dialogVisible"
        :show-close='false'>
        <el-form ref="myForm" :model='formData' :rules='rules' label-width='80px'>
          <el-form-item prop='tagName' label='标签名称'>
            <el-input v-model="formData.tagName"></el-input>
          </el-form-item>
          <el-form-item prop='subjectID' label='学科'>
            <el-select v-model="formData.subjectID">
              <el-option v-for="item in subjects" :key="item.value" :label='item.label' :value='item.value'></el-option>
            </el-select>
          </el-form-item>
        </el-form>
           <el-row slot="footer" type='flex' justify='end'>
              <el-button @click="dialogCancel">取 消</el-button>
              <el-button @click='submitForm' type="primary">确 定</el-button>
           </el-row>
        
      </el-dialog>
    </el-card>
</template>

<script>
import { list, removeState as changeState ,
remove as removeTag,
update as updateTag,
add as addTag,
detail as detailTag} from '../../api/hmmm/tags';
import { simple as getSubjects } from '../../api/hmmm/subjects'
export default {
  name: 'TagsList',
  data() {
    return {
      dialogVisible: false,
      dataList: [], //设置变量接收返回标签数据
      pageObj: {
        total: 0,
        currentPage: 1,
        pageSize: 10
      },
      searchForm: {
        tagName: ''
      },
      formData: {
        subjectID: '',
        tagName: ''
      }, //对话框数据
      rules: {
        // 校验规则中加入trigger后 可使共同使用一个表单组件 切换时不会直接校验 只有输入内容离开后才会校验
        subjectID: [{required: true, message: '学科id不能为空',trigger: 'blur'}],
        tagName: [{required: true, message: '标签名称不能为空',trigger: 'blur'}]
      },
      subjects: [] //接收返回的学科数据
    }
  },
  methods: {
    // 点击修改
    async modify (id) {
      this.dialogVisible = true
      // 将该id的标签内容显示到 对话框表单中 即将获取数据给表单数据对象
       let res = await detailTag({id});
        this.formData = res.data
    },
    // 提交 新增/修改标签数据
    async submitForm () {
      // 先手动校验
      await this.$refs.myForm.validate();
        // 校验成功 判断是新增还是修改 发送请求 增加数据
        // 由于新增时 直接填入数据 so只有表单数据
        // 若是修改 需获取特定标签详情 并将返回数据赋给表单数据 
        // 又根据接口 标签详情返回数据中有id so此时表单数据中也会有 而新增时没有
        this.formData.id ? await updateTag(this.formData) :
        await addTag(this.formData);
          this.$message({
            type:'success',
            message: '操作成功'
          });
         // 添加成功后 清除内容 关闭对话框 重新获取数据
        this.dialogCancel()
        this.getDataList(this.searchForm)
    },
    // 对话框取消 清空数据
    dialogCancel () {
      this.formData = {
        subjectID: '',
        tagName: ''
      };
      this.dialogVisible = false //关闭弹层
    },
    // 删除标签
     async deltag (id) {
      // 确认删除
       try {
        await this.$confirm('您是否确定要删除此数据？');
         await removeTag ({id});
      this.$message({ type: 'success', message: '删除成功' });
      this.getDataList(this.searchForm) //重新带条件查询数据
      } catch (error) {
        this.$message({
            type: 'info',
            message: '已取消删除'
      })
      }
    },
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
    },
    // 获得简单学科列表
    getSubjects () {
      getSubjects().then(res => {
        this.subjects = res.data
      })
    }
  },
  created () {
    this.getDataList()
    this.getSubjects()
  }
}
</script>

<style scoped>
</style>
