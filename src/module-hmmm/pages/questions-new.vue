<template>
<!--  <div class="dashboard-container">-->
<!--    <div class="app-container">题库添加</div>-->


<!--  </div>-->
    <div class="dashboard-container">
        <div class="app-container">
            <el-card shadow="never">
                <!-- 标题 开始-->
                <bread-crumb >
                    <template >
                        试题录入
                    </template>
                </bread-crumb>
                <!--标题 结束 -->
                <!-- 容器 -->
                <el-form  label-width="100px">
<!--                    <el-form-item prop="title" label="标题">-->
<!--                        <el-input  style="width:30%"></el-input>-->
<!--                    </el-form-item>-->
                    <!--   选择-->
                    <el-form-item label="*学科">
                        <el-select  placeholder="请选择">
                            <el-option label="区域一" value="shanghai"></el-option>
                            <el-option label="区域二" value="beijing"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="*目录">
                        <el-select  placeholder="请选择">
                            <el-option label="区域一" value="shanghai"></el-option>
                            <el-option label="区域二" value="beijing"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="*企业">
                        <el-select  placeholder="请选择">
                            <el-option label="区域一" value="shanghai"></el-option>
                            <el-option label="区域二" value="beijing"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="*城市">
                        <el-select  style="width: 15%" placeholder="请选择">
                            <el-option label="区域一" value="shanghai"></el-option>
                            <el-option label="区域二" value="beijing"></el-option>
                        </el-select>
                        -
                        <el-select  style="width: 15%" placeholder="请选择">
                            <el-option label="区域一" value="shanghai"></el-option>
                            <el-option label="区域二" value="beijing"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="*方向">
                        <el-select  placeholder="请选择">
                            <el-option label="区域一" value="shanghai"></el-option>
                            <el-option label="区域二" value="beijing"></el-option>
                        </el-select>
                    </el-form-item>
                    <!--   选择 结束-->
                     <!--  多选-->
                    <el-form-item    label="*题型">
                        <el-radio-group >
                            <!-- // 封面类型 -1:自动，0-无图，1-1张，3-3张 -->
                            <el-radio >单选</el-radio>
                            <el-radio >多选</el-radio>
                            <el-radio >问答</el-radio>
<!--                            <el-radio :label="-1">自动</el-radio>-->
                        </el-radio-group>
                    </el-form-item>
                    <el-form-item    label="*题型">
                        <el-radio-group >
                            <!-- // 封面类型 -1:自动，0-无图，1-1张，3-3张 -->
                            <el-radio >简单</el-radio>
                            <el-radio >一般</el-radio>
                            <el-radio >困难</el-radio>
                            <!--                            <el-radio :label="-1">自动</el-radio>-->
                        </el-radio-group>
                    </el-form-item>
                    <!-- 题干-->
                    <el-form-item prop="content" label="*题干">
                       <el-input type="textarea" :rows="4">

                       </el-input>
                    </el-form-item>
                    <!-- 题干 end-->
                    <!--   上传按钮-->
                    <el-form-item  style="margin-left: -50px;">
                    <el-upload
                            class="upload-demo"
                            action="https://jsonplaceholder.typicode.com/posts/"
                            :on-preview="handlePreview"
                            :on-remove="handleRemove"
                            :before-remove="beforeRemove"
                            multiple
                            :limit="3"
                            :on-exceed="handleExceed"
                            :file-list="fileList">
                        <span style="width: 10%">【解析视频】：</span>
                        <el-button size="small" type="primary">上传图片</el-button>
                        <div slot="tip" class="el-upload__tip" style="display: inline-block">未选择任何文件</div>
                    </el-upload>
                        </el-form-item >
                       <!-- 答案解析-->
                    <el-form-item prop="content" label="*答案解析">
                        <el-input type="textarea" :rows="4">

                        </el-input>
                    </el-form-item>
                    <!-- 答案解析 end-->
                    <!--  题目备注-->
                    <el-form-item prop="content" label="*题目备注">
                        <el-input type="textarea" :rows="4">

                        </el-input>
                    </el-form-item>
                    <!--  题目备注 end-->
                    <el-form-item label="*试题标签">
                        <el-select  placeholder="请选择">
                            <el-option label="区域一" value="shanghai"></el-option>
                            <el-option label="区域二" value="beijing"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item prop="content" label="内容">
                        <quill-editor style="height:300px"></quill-editor>
                    </el-form-item>
                    <el-form-item  >
                        <el-button @click="publishArticle()" type='primary'>提交</el-button>
                        <el-button @click="publishArticle(true)">取消</el-button>
                        </el-form-item>
                </el-form>
                </el-card>
               </div>
    </div>

</template>

<script>
export default {
  data() {
    return {}
  }
}
</script>

<style scoped>
</style>
