<template>
  <div class="dashboard-container">
    <!-- <div class="app-container">题库-精选</div> -->
    <el-card class="header">
      <el-button type="primary">新增试题</el-button>
      <el-button type="primary">批量导入</el-button>
    </el-card>
    <el-card class="banner">
      <el-form>
        <el-form-item class="el-form-item" label="学科" >
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shangh"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="状态">
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shanghai"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="难度">
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shanghai"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="试题类型">
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shanghai"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="标签">
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shanghai"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="关键字">
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shanghai"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="题目备注">
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shanghai"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="企业简称">
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shanghai"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="录入人">
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shanghai"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="二级目录">
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shanghai"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="城市">
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shanghai"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="区域">
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shanghai"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="el-form-item" label="方向">
          <el-select placeholder="请选择" value="">
            <el-option label="区域一" value="shanghai"></el-option>
          </el-select>
        </el-form-item>
        <el-row class="clear">
          <el-button >清除</el-button>
          <el-button type="primary">搜索</el-button>
        </el-row>
        <el-row>
          <el-radio-group v-model="radio1">
      <el-radio-button label="上海"></el-radio-button>
      <el-radio-button label="北京"></el-radio-button>
      <el-radio-button label="广州"></el-radio-button>
    </el-radio-group>
        </el-row>
        <el-table  style="width: 100%" height="250">
          <el-table-column fixed prop="date" label="序号" ></el-table-column>
          <el-table-column prop="name" label="试题编号" ></el-table-column>
          <el-table-column prop="province" label="学科" ></el-table-column>
          <el-table-column prop="city" label="题型" ></el-table-column>
          <el-table-column prop="address" label="题干" ></el-table-column>
          <el-table-column prop="zip" label="录入时间" ></el-table-column>
          <el-table-column prop="zip" label="录入人" ></el-table-column>
          <el-table-column prop="zip" label="难度" ></el-table-column>
          <el-table-column prop="zip" label="审核状态" ></el-table-column>
          <el-table-column prop="zip" label="审核意见" ></el-table-column>
          <el-table-column prop="zip" label="审核人" ></el-table-column>
          <el-table-column prop="zip" label="发布状态" ></el-table-column>
        </el-table>
      </el-form>
    </el-card>
  </div>
</template>

<script>
export default {
  name: "QuestionsChoice",
  data() {
    return {
      radio1:'shangh'
    };
  }
};
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
