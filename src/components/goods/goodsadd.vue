<template>
    <div class="tmpl">
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
            <el-form-item label="商品标题" prop="title">
                <el-input v-model="ruleForm.title"></el-input>
            </el-form-item>

            <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
                <el-button @click="resetForm('ruleForm')">重置</el-button>
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
                rules: {
                    title: [
                        // 表示商品标题非空验证，通过失去焦点的时候触发
                        { required: true, message: '请输入商品标题', trigger: 'blur' },
                    ]
                }
            }
        },
        methods: {
            // 1.0 重置表单
            resetForm(formName) {
                this.$refs[formName].resetFields();
            },
            // 2.0 提交保存
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    // valid:返回true的话，表示表单元素全部合法，否则不合法
                    if (valid) {
                        alert('提交成功');
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