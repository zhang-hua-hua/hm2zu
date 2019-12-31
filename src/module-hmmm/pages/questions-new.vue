<!--参考老师 继续优化-->
<template>
    <el-card>
        <el-form
                :rules="rules"
                :model="formData"
                ref="myForm"
                style="margin-left:100px"
                label-width="80px"
        >
            <el-form-item label="学科" prop="subjectID">
                <el-select v-model="formData.subjectID" style="width:40%">
                    <el-option
                            v-for="item in subjects"
                            :key="item.value"
                            :value="item.value"
                            :label="item.label"
                    ></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="目录" prop="catalogID">
                <el-select v-model="formData.catalogID" style="width:40%">
                    <el-option
                            v-for="(item) in directioy"
                            :key="item.id"
                            :value="item.value"
                            :label="item.label"
                    ></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="企业" prop="enterpriseID">
                <el-select v-model="formData.enterpriseID">
                    <el-option
                            v-for="item in companys"
                            :key="item.id"
                            :label="item.shortName"
                            :value="item.id"
                    ></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="城市" prop="province" style="width:40%">
                <el-select v-model="formData.province" style="width:45%">
                    <el-option v-for="(item,index) in provinces" :key="index" :label="item" :value="item"></el-option>
                </el-select>
                <el-select style="width:45%;margin-left:20px" v-model="formData.city">
                    <el-option v-for="(item,index) in myCitys" :key="index" :label="item" :value="item"></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="方向" prop="direction">
                <el-select v-model="formData.direction" style="width:40%">
                    <el-option v-for="(item,index) in direction" :key="index" :value="item" :label="item"></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="题型" prop="questionType">
                <el-radio-group v-model="formData.questionType">
                    <el-radio label="1">单选</el-radio>
                    <el-radio label="2">多选</el-radio>
                    <el-radio label="3">简答</el-radio>
                </el-radio-group>
            </el-form-item>
            <el-form-item label="难度" prop="difficulty">
                <el-radio-group v-model="formData.difficulty">
                    <el-radio label="1">简单</el-radio>
                    <el-radio label="2">一般</el-radio>
                    <el-radio label="3">困难</el-radio>
                </el-radio-group>
            </el-form-item>
            <el-form-item prop="question" label="题干">
                <quill-editor
                        style="width:60%;height:300px;margin-bottom: 100px"
                        v-model="formData.question"
                ></quill-editor>
            </el-form-item>
            <template v-if="formData.questionType!=='3'">
                <el-form-item label="选项(以下选中的选项为正确答案)"></el-form-item>
                <el-form-item v-for="(item,index) in formData.options" :key="index">
                    <!-- 单选 -->
                    <el-radio
                            @change="selectThis(index)"
                            v-if="formData.questionType === '1'"
                            :label="1"
                            v-model="item.isRight"
                    >{{item.code }}</el-radio>
                    <el-checkbox
                            v-else-if="formData.questionType === '2'"
                            :true-label="1"
                            :false-label="0"
                            v-model="item.isRight"
                            :label="item.code"
                    ></el-checkbox>

                    <el-input style="width:40%;margin-left:10px" v-model="item.title"></el-input>
                    <el-button
                            size="small"
                            icon="el-icon-delete"
                            style="margin-left:10px"
                            @click="delItem(index)"
                    >删除该选项</el-button>
                </el-form-item>
            </template>
            <el-form-item>
                <el-button type="primary" @click="addItem" v-if="formData.questionType!=='3'">增加选项与答案</el-button>
            </el-form-item>
            <el-form-item label="解析视频" prop="videoURL">
                <el-input v-model="formData.videoURL" style="width:40%"></el-input>
            </el-form-item>
            <el-form-item label="答案解析" prop="answer">
                <quill-editor style="width:60%;height:300px;margin-bottom: 100px" v-model="formData.answer"></quill-editor>
            </el-form-item>
            <el-form-item label="题目备注" prop="remarks">
                <el-input v-model="formData.remarks" style="width:40%"></el-input>
            </el-form-item>
            <el-form-item label="试题标签" prop="tags">
                <quill-editor style="width:60%;height:300px;margin-bottom: 100px" v-model="formData.tags"></quill-editor>
            </el-form-item>
        </el-form>
        <el-row type="flex" justify="center">
            <el-button type="primary" @click="submit">提交</el-button>
            <el-button @click="clear">清空</el-button>
        </el-row>
    </el-card>
</template>

<script>
    import { add as addQuestion, detail, update } from "../../api/hmmm/questions";
    import { simple as TagList } from "../../api/hmmm/tags";
    import { difficulty, questionType, direction } from "../../api/hmmm/constants";
    import { simple as subjectList } from "../../api/hmmm/subjects";
    import { simple as directioy } from "../../api/hmmm/directorys";
    import { citys, provinces } from "../../api/hmmm/citys";
    import { quillEditor } from "vue-quill-editor";
    import { list as companys } from "../../api/hmmm/companys";
    import "quill/dist/quill.core.css";
    import "quill/dist/quill.snow.css";
    import "quill/dist/quill.bubble.css";
    export default {
        components: {
            quillEditor
        },
        data() {
            return {
                dic: "abcdefghijklmnopqrstuvwxyz",
                list: [],
                subjects: [],
                tags: [],
                direction,
                difficulty,
                questionType,
                directioy,
                citys: [],
                provinces: provinces(),
                users: [],
                companys: [],
                formData: {
                    options: [],
                    videoURL: "",
                    answer: "",
                    question: "",
                    difficulty: "1",
                    questionType: "1",
                    tags: null,
                    subjectID: null,
                    province: null,
                    city: null,
                    keyword: null,
                    remarks: "",
                    shortName: null,
                    direction: null,
                    catalogID: null,
                    enterpriseID: null
                },
                rules: {
                    question: [{ required: true, message: "题干不能为空" }],
                    tags: [{ required: true, message: "标签不能为空" }],
                    enterpriseID: [{ required: true, message: "企业不能为空" }],
                    subjectID: [{ required: true, message: "学科不能为空" }],
                    province: [{ required: true, message: "城市不能为空" }],
                    city: [{ required: true, message: "区域不能为空" }],
                    answer: [{ required: true, message: "答案解析不能为空" }],
                    direction: [{ required: true, message: "方向不能为空" }],
                    catalogID: [{ required: true, message: "目录不能为空" }],
                    remarks: [{ required: true, message: "试题标签不能为空" }],
                    videoURL: [{ required: true, message: "视频解析不能为空" }]
                }
            };
        },
        computed: {
            myCitys() {
                return citys(this.formData.province);
            }
        },
        methods: {
            selectThis(index) {
                let options = this.formData.options.map((item, i) => {
                    i === index ? (item.isRight = 1) : (item.isRight = 0);
                    return item;
                });
                console.log(options);
            },
            addItem() {
                this.formData.options.push({
                    code: this.getRightLetter(this.formData.options.length),
                    title: "",
                    isRight: false,
                    img: ""
                });
            },
            async delItem(index) {
                await this.$confirm("是否删除此选项?");
                this.formData.options.splice(index, 1);
            },
            getRightLetter(index) {
                return this.dic.split("")[index].toUpperCase();
            },
            clear() {
                this.$refs.myForm.resetFields();
            },
            async submit() {
                await this.$refs.myForm.validate();
                let { id } = this.$route.query;
                id ? await update(this.formData) : await addQuestion(this.formData);

                this.$router.push("/questions/list");
            },
            async getSubject() {
                let result = await subjectList();
                this.subjects = result.data;
            },
            async getTags() {
                let result = await TagList();
                this.tags = result.data;
            },
            async getDirectioy() {
                let result = await directioy();
                this.directioy = result.data;
            },
            async getCompanys() {
                let result = await companys();
                this.companys = result.data.items;
            },
            async getDetail(id) {
                let result = await detail({ id });
                this.formData = result.data;
            }
        },
        async created() {
            this.getCompanys();
            this.getDirectioy();
            this.getTags();
            this.getSubject();
            let { id } = this.$route.query;
            id && this.getDetail(id);
        }
    };
</script>

<style scoped>

</style>
