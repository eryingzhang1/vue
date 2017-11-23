<template>
    <div class="tmpl">
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
            <el-form-item label="商品标题" prop="title">
                <el-input v-model="ruleForm.title"></el-input>
            </el-form-item>

        </el-form>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                // 表单元素的双向数据绑定对象
                ruleForm: {
                    title: ''
                },
                // 表单元素的验证规则
                rules:{
                    title:[
                        // 表示商品标题非空验证，通过失去焦点的时候触发
                        { required: true, message: '请输入商品标题', trigger: 'blur' },
                    ]
                }
            }
        },
        methods: {
        }
    }
</script>
<style scoped>
</style>