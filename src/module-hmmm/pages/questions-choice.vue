<template>
  <div class="dashboard-container">
    <!-- <div class="app-container">题库-精选</div> -->
    <el-card class="header">
       <el-button @click="$router.push('/questions/new')" type="primary">新增试题</el-button>
      <el-button type="primary">批量导入</el-button>
    </el-card>
    <el-card class="banner">
      <el-form :inline="true"  :model="serchForm" ref="myForm">
        <el-form-item class="el-form-item" label="学科" prop="subject" >
          <el-select v-model="serchForm.subject">
            <el-option v-for="item in subjects" :key="item.value" :value="item.value" :label="item.label" ></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="状态" prop="statu" >
          <el-select v-model="serchForm.statu">
            <el-option v-for="item in status"
             :key="item.value" 
             :value="item.value" 
             :label="item.label">
             </el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="难度" prop="difficult" >
          <el-select v-model="serchForm.difficult">
            <el-option v-for="item in difficulty" :key="item.value" :value="item.value" :label="item.label"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="试题类型" prop="questionType">
          <el-select  v-model="serchForm.questionType" >
            <el-option v-for="item in questionType" :key="item.value" :value="item.value" :label="item.label"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="标签" prop="Label">
          <el-select v-model="serchForm.Label"  >
            <el-option  v-for="item in Label" :key="item.value" :value="item.value" :label="item.label"></el-option>
          </el-select>
        </el-form-item>
      <el-form-item label="关键字" prop="keyword">
        <el-input v-model="serchForm.keyword"></el-input>
      </el-form-item>
      <el-form-item label="题目备注" prop="remarks">
        <el-input v-model="serchForm.remarks"></el-input>
      </el-form-item>
      <el-form-item label="企业简称" prop="shortName">
        <el-input v-model="serchForm.shortName"></el-input>
      </el-form-item>
        <el-form-item class="el-form-item" label="录入人"  prop="creatorID">
          <el-select placeholder="请选择" v-model="serchForm.creatorID">
            <el-option v-for="(item) in users" :key="item.id" :value="item.id" :label="item.username"></el-option>
          </el-select>
        </el-form-item>
       <el-form-item label="二级目录" prop="catalogID">
        <el-select v-model="serchForm.catalogID">
            <el-option  v-for="(item) in directioy"
            :key="item.id"
            :value="item.value"
            :label="item.label"></el-option>
          </el-select>
        </el-form-item>
        
        <el-form-item class="el-form-item" label="城市" prop="province">
          <el-select  placeholder="请选择" v-model="serchForm.province">
            <el-option  v-for="(item,index) in provinces" :key="index" :value="item" :label="item"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="区域" prop="city">
          <el-select placeholder="请选择" v-model="serchForm.city">
            <el-option v-for="(item,index) in myCitys" :key="index" :label="item" :value="item"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="方向" prop="direction">
            <el-select v-model="serchForm.direction">
              <el-option v-for="(item,index) in direction" :key="index" :value="item" :label="item"></el-option>
            </el-select>
        </el-form-item>
        
        <el-row class="clear">
              <el-button @click="clear">清除</el-button>
             <el-button @click="search" type="primary">搜索</el-button>
        </el-row>
        <el-row>
              <el-radio-group v-model="radio1">
                <el-radio-button label="全部"></el-radio-button>
                <el-radio-button label="待发布"></el-radio-button>
                <el-radio-button label="已发布"></el-radio-button>
              </el-radio-group>
        </el-row>


        <el-table  :data="choice.items" style="width: 100%" >
          <el-table-column fixed prop="id" label="序号" ></el-table-column>
          <el-table-column prop="number" label="试题编号" ></el-table-column>
          <el-table-column prop="subjectID" label="学科" ></el-table-column>
          <el-table-column prop="questionType" label="题型" ></el-table-column>
          <el-table-column prop="question" label="题干" ></el-table-column>
          <el-table-column prop="addDate" label="录入时间" >
            <span slot-scope="obj">{{obj.row.addDate| parseTimeByString }}</span>
          </el-table-column>
          <el-table-column prop="creator" label="录入人" ></el-table-column>
          <el-table-column prop="difficulty" label="难度" ></el-table-column>
          <el-table-column prop="chkState" label="审核状态" ></el-table-column>
          <el-table-column prop="chkRemarks" label="审核意见" ></el-table-column>
          <el-table-column prop="chkUser" label="审核人" ></el-table-column>
          <el-table-column prop="publishState" label="发布状态" ></el-table-column>
          <el-table-column prop="" label="操作" width="220px" align="center">

            <el-button type="text">审核</el-button>
            <el-button type="text">预览</el-button>
            <el-button type="text">上架</el-button>
            <el-button type="text">修改</el-button>
            <el-button type="text">删除</el-button>
          </el-table-column>
         
        </el-table>
         
          <el-row type="flex" justify="end" style="margin:30px 0">
                <el-pagination background 
                  :page-size="page.pageSize"
                  :current-page="page.currentPage"
                  layout="total, prev, pager, next"
                  :total="page.total"
                  @current-change="changePage">
                 </el-pagination>
          </el-row>
     </el-form>
    </el-card>
  </div>
</template>

<script>
import {simple as subjects } from "../../api/hmmm/subjects"  //简单学科
import{choice as questionList,} from "../../api/hmmm/questions"    //数据
import {simple as UserList } from "../../api/base/users"//用户状态
import{difficulty} from '../../api/hmmm/constants' //难度
import{questionType} from '../../api/hmmm/constants' //试题类型
import{simple as Label} from "../../api/hmmm/tags" //标签
import{provinces} from "../../api/hmmm/citys"  // 所有城市
import{citys} from "../../api/hmmm/citys"      // 所有 城市区
import { simple as directioy } from "../../api/hmmm/directorys" //二级目录
import { direction} from '../../api/hmmm/constants' //方向

export default {
  name: "QuestionsChoice",
  data () {
    return {
      serchForm:{
        shortName:null,
        remarks:null,
        keyword:null,
        subject:null, //学科名称
        statu:null,       //状态 es7 简写
        difficult:null,//难度 es7 简写
        questionType:null, // 实体类型 es7 简写
        Label:null,   //标签
        direction:null,
        province:null,
        creatorID:null,
        catalogID:null,
        city: null,
      },
      radio1:'shangh',
       page:{
        currentPage:1,//当前页码
        pageSize:10 , //每页条数
        total:0 //总数量
      },
      status: [
        {
          value: 0,
          label: "待审核"
        },
        {
          value: 1,
          label: "通过"
        },
        {
          value: 2,
          label: "拒绝"
        }
      ],//状态数据
      provinces:provinces(),
      citys:[],
      choice:[],  //列表数据   
      Label,
      subjects,
      users:[],
      difficulty,//难度 es7 简写
      questionType, // 实体类型 es7 简写
      directioy,
      direction,
    };
  },

methods:{
   getCondition() {
      let params = {
        page: this.page.currentPage,
        pageSize: this.page.pageSize,
        ...this.serchForm
      };
      this.getQuestion(params);
    },
  search() {
      this.page.currentPage = 1;
      this.getCondition();
    },
   clear() {
      this.$refs.myForm.resetFields();
    },
   myCitys() {
      return citys(this.serchForm.province);
    },
   async getUserList() {
      let result = await UserList();
      this.users = result.data;
    },
      async getDirectioy() {
      let result = await directioy();
      this.directioy = result.data;
    },
     changePage(newPage) {
      this.page.currentPage = newPage;
      this.getCondition();
    },
    getCondition() {
      let params = {
        page: this.page.currentPage,
        pageSize: this.page.pageSize,
        ...this.serchForm
      };
        this.getQuestion(params);
    },
     search() {
      this.page.currentPage = 1;
      this.getCondition();
    },
    async getQuestion(data) {
      let result = await questionList(data);
      this.list = result.data.items;
      this.page.total = result.data.counts;
    },
},
  created(){
      this.getQuestion();
     this.getUserList();
     this.getDirectioy();
    questionList().then(result=>{
      this.choice=result.data
    })
    subjects().then(result=>{
      this.subjects =result.data
    })
    Label().then(result=>{
      this.Label = result.data
    })
    provinces().then(result=>{
      this.provinces=result.data
    })
    citys().then(result=>{
      this.citys=result.data
    })
    // UserList().then(result=>{
    //   this.users = result.data;
    // })
      
  
      
    

  },
  
}
</script>

<style scoped lang="scss">
.el-form{
  .el-form-item {
  display: inline-block;
  width: 300px;
}
  .clear{
    display: flex;
    justify-content: center;
  }
}

// 1. 如果在此处 scoped 当前组件下生效样式
// 2. 在style中的所有选择器 编译的时候会自动带上属性选择器
// 3. .box{} ===> .box[data-v-2c9827a4]{} 交集选择器
// 4. 在当前组件下暴露的标签都会加上 data-v-2c9827a4 属性
// 5. 但是如果是组件，其他组件内部的标签是不会加上这个属性 意味写组件内部的样式是不会生效的
// 6. 其他组件的样式都不会生效
</style>
