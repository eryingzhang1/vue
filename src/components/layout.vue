<template>
    <div class="tmpl">

        <el-row>
            <el-col :span="4">
                <!-- 1.0 左边菜单 -->
                <img src="../../statics/imgs/logo.png" alt="">
                <div class="layout menu">
                    <el-menu default-active="1-2" class="el-menu-vertical-demo" @open="handleOpen" @close="handleClose">
                        <el-submenu index="1">
                            <template slot="title">
                                <i class="el-icon-message"></i>购物商城</template>
                            <el-menu-item-group>                               
                                <el-menu-item index="1-1">内容管理</el-menu-item>
                                <el-menu-item index="1-2">分类管理</el-menu-item>
                                <el-menu-item index="1-3">评论管理</el-menu-item>
                            </el-menu-item-group>                           
                        </el-submenu>
                        <el-submenu index="2">
                                <template slot="title">
                                    <i class="el-icon-message"></i>订单管理</template>
                                <el-menu-item-group>                               
                                    <el-menu-item index="2-1">订单列表</el-menu-item>
                                    <el-menu-item index="2-2">分类管理</el-menu-item>
                                    <el-menu-item index="2-3">评论管理</el-menu-item>
                                </el-menu-item-group>                           
                            </el-submenu>
                    </el-menu>
                </div>
            </el-col>
            <el-col :span="20">
                <el-row>
                    <el-col :span="24">
                        <!--2.0 顶部登录信息条 -->
                        顶部
                    </el-col>
                </el-row>

                <el-row>
                    <el-col :span="24">
                        <!-- 3.0 其他功能的渲染区占位符 -->
                        其他功能的渲染区占位符
                        <router-view></router-view>

                    </el-col>
                </el-row>
            </el-col>
        </el-row>

    </div>
</template>

<script>

    export default {
        data() {
            return {
            }
        },
        methods: {
        }
    }
</script>
<style scoped>
</style>