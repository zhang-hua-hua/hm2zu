<template>
  <!-- 整个容器 -->
  <el-card>
    <el-row slot="header">
      <el-button type="primary" @click="dialogVisible = true">新增目录</el-button>
      <el-button type="primary">返回学科</el-button>
    </el-row>
    <!-- 表单 -->
    <el-form inline>
      <el-form-item label="目录名称">
        <el-input v-model="searchForm.directoryName"></el-input>
      </el-form-item>
      <el-form-item label="状态">
        <el-select v-model="searchForm.state">
          <el-option v-for="item in status" :key="item.value" :label="item.label" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button @click="clear">清除</el-button>
        <el-button @click="search" type="primary">搜索</el-button>
      </el-form-item>
    </el-form>
    <!-- 表格 -->
    <el-table :data="list">
      <el-table-column prop="id" label="序号"></el-table-column>
      <el-table-column prop="directoryName" label="目录名称"></el-table-column>
      <el-table-column prop="creator" label="创建者"></el-table-column>
      <el-table-column align="center" prop="addDate" label="创建时间">
        <!-- 过滤器 表达式 | 过滤器名称 -->
        <template slot-scope="obj">{{ obj.row.addDate | parseTimeByString }}</template>
      </el-table-column>
      <el-table-column align="center" prop="totals" label="面试题数量"></el-table-column>
      <el-table-column prop="state" label="状态">
        <template slot-scope="obj">{{ obj.row.state === 1 ? '开启' : '禁用' }}</template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="obj">
          <el-button type="text" @click="modify(obj.row.id)">修改</el-button>
          <el-button type="text" @click="changeState(obj.row)">{{ obj.row.state === 1 ? '禁用' : '开启' }}</el-button>
          <el-button type="text" @click="delItem(obj.row.id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 分页符 -->
    <el-row type="flex" justify="end" style="margin:30px 0">
      <el-pagination background layout="prev, pager, next" 
      :total="page.total"
      :page-size="page.pageSize"
      :current-page="page.currentPage"
      @current-change="currentChange"
      ></el-pagination>
    </el-row>
    <el-dialog :visible="dialogVisible" :show-close="false" :destroy-on-close="true">
      <el-form ref="myForm" :model="formData" :rules="rules" label-width="80px">
        <el-form-item label="目录名称" prop="directoryName">
          <el-input v-model="formData.directoryName" style="width:30%"></el-input>
        </el-form-item>
        <el-form-item label="学科" prop="subjectID">
          <el-select v-model="formData.subjectID" style="width:30%">
            <el-option v-for="item in subjects" :key="item.value" :label="item.label" :value="item.value"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <el-row type="flex" justify="end" slot="footer">
        <el-button type="primary" @click="submitForm">确定</el-button>
        <el-button @click="closeMip">取消</el-button>
      </el-row>
    </el-dialog>
  </el-card>
</template>

<script>
import { list as DirectorysList,
remove as removeDirectory,
removeState as changeState,
add as addDirectory,
detail as detailDirectory,
update as updateDirectory
} from '../../api/hmmm/directorys'
import { status } from '../../api/hmmm/constants'
import { simple as getSubjects } from '../../api/hmmm/subjects'
export default {
  name: 'DirectorysList',
  data() {
    return {
      // 弹层
      dialogVisible: false, // 弹窗默认隐藏
      // 搜索对象
      searchForm:{
        directoryName: '', // 目录名称默认为空
        state: null // 状态默认无状态
      },
      // 列表数据
      list:[],
      // 分页数据
      page:{
        total:0, // 总条目数
        currentPage: 1, // 当前页码
        pageSize: 10 // 每页显示条目个数
      },
      // 状态数据
      status,
      // 弹窗表单数据
      formData:{
        subjectID: '', // 学科ID
        directoryName: '' // 目录名称
      },
      // 校验规则
      rules:{
        // trigger: ""   blur 失去焦点触发      change  数据改变触发
        subjectID:[{ required: true, message: "学科id不能为空" }],
        directoryName:[{ required: true, message: "目录名称不能为空" }]
      },
      subjects: [] // 用来接收科目数据,result.data进行中转
    }
  },
  methods:{
    // 分页
    currentChange(newPage) {
      this.page.currentPage = newPage;
      this.getDirectorys(this.searchForm);
    },
    // 修改数据
    async modify(id) {
      // 调用修改接口
      let result = await detailDirectory({ id });
      this.formData = result.data;
      this.dialogVisible = true
    },
    // 关闭弹层
    closeMip() {
      this.dialogVisible = false,
      // 回到初始状态
      this.formData = {
        subjectID: '', // 学科ID
        directoryName: '' // 目录名称
      }
    },
    // 手动校验表单并且提交
    async submitForm() {
      await this.$refs.myForm.validate();
      // 校验成功之后 调用新增接口
     this.formData.id ? await updateDirectory(this.formData) : await addDirectory(this.formData);
      // 操作成功后进行提示
      this.$message({ type: "success", message: "操作成功" });
      // 关闭弹层前需要清空formData中的数据
      this.formData = {
        subjectID: '', // 学科ID
        directoryName: '' // 目录名称
      };
      // 关闭弹层
      this.dialogVisible = false;
      this.getDirectorys(this.searchForm);
    },
    // 切换状态
    async changeState(row) {
      await this.$confirm("您是否确定删除此状态?");
      await changeState({ id:row.id, state: row.state === 1 ? 0 : 1 });
      this.$message({ type:'success', message:'改变状态成功'});
      this.getDirectorys(this.searchForm); // this.searchForm 带状态查询
    },
    // 删除
    async delItem(id) {
      await this.$confirm("您是否确定删除此数据?");
      await removeDirectory({ id });
      this.$message({ type:'success', message:'删除成功'});
      this.getDirectorys(this.searchForm); // this.searchForm 带状态查询
    },
    // 搜索
    search() {
      this.page.currentPage=1;
      this.getDirectorys(this.searchForm)
    },
    // 清除
    clear() {
      // 将数据更改为初始状态
      this.searchForm = {
        directoryName: '', // 目录名称默认为空
        state: null // 状态默认无状态
      }
    },
    // 获取目录数据
    async getDirectorys(data){
      // 调用list接口
      let result = await DirectorysList({
        page: this.page.currentPage,
        pagesize: this.page.pageSize,
        ...data
      });
      this.list = result.data.items; // 列表数据
      this.page.total = result.data.counts; // 总条数
    },
    // 获取学科方法
    async getSubjects() {
    let result = await getSubjects();
    this.subjects = result.data
    }
  },
  created() {
    this.getDirectorys(); // 调用getDirectorysList这个方法 调用学科
    this.getSubjects(); // 调用getSubjects这个方法
  }
}
</script>

<style scoped>
</style>
