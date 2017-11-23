<template>
    <div class="tmpl">
        <!--1.0 面包屑-->
        <div class="abread bt-line">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item>
                    <span>返回上一层</span>
                </el-breadcrumb-item>
                <el-breadcrumb-item>购物商城</el-breadcrumb-item>
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>新增商品</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="operation">
            <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
                <el-row>
                    <el-col :span="18">
                        <el-form-item label="商品标题" prop="title">
                            <el-input v-model="ruleForm.title"></el-input>
                        </el-form-item>
                        <el-form-item label="副标题" prop="sub_title">
                            <el-input v-model="ruleForm.sub_title"></el-input>
                        </el-form-item>
                        <el-form-item label="所属类别">
                            <el-select v-model="ruleForm.category_id" placeholder="所属类别">
                                <!-- 这些值一定是从数据api接口请求下来的 v-for来生成  el-option-->
                                <el-option v-for="item in catelist" :value="item.category_id" :label="item.title">
                                    <!-- 判断item.class_layer >1 就应该添加 |- -->
                                    <span v-for="sub in (item.class_layer -1 )">&nbsp;</span>
                                    <span v-if="item.class_layer>1">|-</span>{{item.title}}
                                </el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="是否发布">
                            <el-switch v-model="ruleForm.status" :width="58" on-text="ON" off-text="OFF" on-color="#13ce66" off-color="#ff4949">
                            </el-switch>
                        </el-form-item>
                        <el-form-item label="推荐类型">
                            <el-switch v-model="ruleForm.is_slide" :width="58" on-text="ON" off-text="OFF" on-color="#13ce66" off-color="#ff4949">
                            </el-switch>
                            <el-switch v-model="ruleForm.is_top" :width="58" on-text="ON" off-text="OFF" on-color="#13ce66" off-color="#ff4949">
                            </el-switch>
                            <el-switch v-model="ruleForm.is_hot" :width="58" on-text="ON" off-text="OFF" on-color="#13ce66" off-color="#ff4949">
                            </el-switch>
                        </el-form-item>
                        <el-form-item label="封面上传">
                            <el-upload class="upload-demo"
                             action="http://127.0.0.1:8899/admin/article/uploadimg"
                              :file-list="ruleForm.imgList" list-type="picture"
                              :on-success="imgUploaded">
                                <el-button size="small" type="primary">点击上传</el-button>
                            </el-upload>
                        </el-form-item>
                        <el-form-item label="相册上传">
                            <el-upload class="upload-demo"
                            action="http://127.0.0.1:8899/admin/article/uploadfile" 
                            :file-list="ruleForm.fileList"
                            :on-success="fileUplaoded"
                            list-type="picture">
                                <el-button size="small" type="primary">点击上传</el-button>
                            </el-upload>
                        </el-form-item>
                        <el-form-item label="商品货号" prop="goods_no">
                            <el-input v-model="ruleForm.goods_no"></el-input>
                        </el-form-item>
                        <el-form-item label="库存数量" prop="stock_quantity">
                            <el-input v-model="ruleForm.stock_quantity"></el-input>
                        </el-form-item>
                        <el-form-item label="市场价格" prop="market_price">
                            <el-input v-model="ruleForm.market_price"></el-input>
                        </el-form-item>
                        <el-form-item label="销售价格" prop="sell_price">
                            <el-input v-model="ruleForm.sell_price"></el-input>
                        </el-form-item>
                        <el-form-item label="内容摘要" prop="zhaiyao">
                            <el-input type="textarea" :rows="2" placeholder="请输入摘要" v-model="ruleForm.zhaiyao">
                            </el-input>
                        </el-form-item>
                        <el-form-item label="商品详情" prop="content">
                            <!-- 用的是vue的一个单独的第三方组件：富文本编辑器组件                            
                            vue-quill-editor 使用参考：http://172.16.2.23/vue/vueproject/project_admin.html#2
                            :content  ->类似于 v-model
                        -->
                            <quill-editor :content="ruleForm.content" @change="onEditorChange($event)">
                            </quill-editor>
                        </el-form-item>

                        <el-form-item>
                            <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
                            <el-button @click="resetForm('ruleForm')">重置</el-button>
                        </el-form-item>

                    </el-col>
                </el-row>
            </el-form>
        </div>
    </div>
</template>

<script>

    // 1.0 导入富文本编辑器组件
    import { quillEditor } from 'vue-quill-editor';

    export default {
        components: {
            quillEditor
        },
        data() {
            return {
                // catelist
                catelist: [],  //存储分类下拉框中的数据
                // 表单元素的双向数据绑定对象
                ruleForm: {
                    title: '1',
                    sub_title: '2',
                    "goods_no": "NZ0000000002",
                    category_id: 43,  // 小问题：category_id要是一个数值类型，如果变成字符串类型的话，则页面显示不正常
                    "stock_quantity": 200,
                    "market_price": 1000,
                    "sell_price": 800,
                    "status": true,
                    "is_slide": true,
                    "is_top": false,
                    "is_hot": true,
                    "zhaiyao": "为不凡而生",
                    "content": "<p><strong>产品参数：</strong></p>",
                    "imgList": [
                        // {
                        //     "name": "wTgAWDLpQReTQ-ZOMdlAk4vF.jpg",
                        //     "url": "http://127.0.0.1:8899/imgs/wTgAWDLpQReTQ-ZOMdlAk4vF.jpg",
                        //     "shorturl": "/imgs/wTgAWDLpQReTQ-ZOMdlAk4vF.jpg"
                        // }
                    ],
                    "fileList": [
                        // {
                        //     "name": "HN5d4_wrbsUk5KQNjzYSGGwm.jpg",
                        //     "url": "http://127.0.0.1:8899/imgs/HN5d4_wrbsUk5KQNjzYSGGwm.jpg",
                        //     "shorturl": "/imgs/HN5d4_wrbsUk5KQNjzYSGGwm.jpg"
                        // }
                    ]
                },
                // 表单元素的验证规则
                rules: {
                    title: [
                        // 表示商品标题非空验证，通过失去焦点的时候触发
                        { required: true, message: '请输入商品标题', trigger: 'blur' },
                    ]
                }
            }
        },
        mounted() {
            // 获取商品的分类数据列表
            this.getcastlist();
        },
        methods: {
            // 6.0 fileUplaoded 相册上传成功以后的回调
            fileUplaoded(responese,file,filelist){
                // 将responese通过 this.ruleForm.fileList.push(responese);才能保证多张图片
                this.ruleForm.fileList.push(responese);
            },
            // 5.0 定义一个方法imgUploaded（当服务器成功响应以后回调）
            imgUploaded(responese,file,filelist){
                // 经过查看对比，responese就是一个和imgList数组中的对象格式一致的对象，所以只需要将这个对象使用[responese]
                console.log(responese);
                this.ruleForm.imgList = [responese];               
            },
            // 4.0 注册富文本编辑器的内容改变事件onEditorChange
            onEditorChange(ev) {
                console.log(ev);
                this.ruleForm.content = ev.html;
            },
            // 3.0 获取商品分类数据
            getcastlist() {
                // 请求接口地址
                var url = '/admin/category/getlist/goods';
                // 发出ajax的get请求
                this.$ajax.get(url).then(res => {
                    this.catelist = res.data.message;
                });
            },
            // 1.0 重置表单
            resetForm(formName) {
                this.$refs[formName].resetFields();
            },
            // 2.0 提交保存
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    // valid:返回true的话，表示表单元素全部合法，否则不合法
                    if (valid) {
                        // 表示整个表单验证成功，应该将数据提交给服务器,完成新增工作
                        this.$ajax.post('/admin/goods/add/goods', this.ruleForm)
                            .then(res => {
                                // 提示用户保存结果
                                if (res.data.status == 1) {
                                    // 提醒失败信息(使用了elementUI中的message消息提示)
                                    this.$message({
                                        showClose: true,
                                        message: res.data.message,
                                        type: 'error',
                                        duration:1000
                                    });
                                }else{
                                    // 跳转到列表页面
                                    // this.$route : 代表接收路由对象，可以通过 this.$route.parmas获取参数
                                    // this.$router：代表路由对象在vue上的一个原型对象可以调用他的push()方法完成页面的跳转
                                    this.$router.push({ name: 'goodslist'});
                                }
                            });
                        // console.log(this.ruleForm);
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            }
        }
    }
</script>
<style scoped>
</style>