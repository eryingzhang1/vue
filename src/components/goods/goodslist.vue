<template>
    <div class="tmpl">
        <el-row>
            <el-col :span="24">
                <!-- 面包屑导航组件 -->
                <div class="abread bt-line">
                    <el-breadcrumb separator="/">
                        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                        <el-breadcrumb-item>购物商城</el-breadcrumb-item>
                        <el-breadcrumb-item>商品列表</el-breadcrumb-item>
                    </el-breadcrumb>
                </div>
            </el-col>
        </el-row>
        <el-row>
            <el-col :span="24">
                <!-- 搜索区域 -->
                <div class="abread bt-line">
                    <el-row>
                        <el-col :span="6">
                            <!--按钮和搜索框  -->
                            <el-button size="small">全选</el-button>
                            <el-button size="small">新增</el-button>
                            <el-button size="small">删除</el-button>
                        </el-col>

                        <el-col :offset="14" :span="4">
                            <!-- 搜索框 -->
                            <el-input placeholder="请输入搜索内容" icon="search" v-model="searchValue" 
                            :on-icon-click="getlist">
                            </el-input>
                        </el-col>
                    </el-row>
                </div>
            </el-col>
        </el-row>
        <el-row>
            <el-col :span="24">
                <!-- 表格区域
                :data="list" 一定但是了一个数组，el-table就会自动遍历生成表格的每一个行数据
                -->
                <el-table ref="multipleTable" :data="list" border tooltip-effect="dark" style="width: 100%">
                    <!-- type="selection":这一列是一个checkbox勾选框 -->
                    <el-table-column type="selection" width="55">
                    </el-table-column>
                    <el-table-column prop="title" label="标题">
                    </el-table-column>
                    <el-table-column prop="categoryname" label="所属类别" width="100">
                    </el-table-column>
                    <el-table-column prop="stock_quantity" label="库存" width="100">
                    </el-table-column>
                    <el-table-column prop="market_price" label="市场价" width="100">
                    </el-table-column>
                    <el-table-column prop="sell_price" label="销售价" width="100">
                    </el-table-column>
                    <el-table-column label="属性" width="120">
                        <!-- scope:代表但是当前:data中的指向的某个对象，可以通过 scope -->
                        <template scope="scope">
                            <!-- 取得是list中的对象中的user_name这个属性的值 -->
                            <i v-bind="{class:'el-icon-picture '+(scope.row.is_slide==1?'':'unlinght') }"></i>
                            <i v-bind="{class:'el-icon-upload '+(scope.row.is_top==1?'':'unlinght') }"></i>                           
                        </template>
                    </el-table-column>

                    <!-- template：是属于当前列的模板，可以有程序员自己编写这一列的样式 -->
                    <el-table-column label="操作" width="120">
                        <template scope="scope">
                            <a href="#">修改</a>
                        </template>
                    </el-table-column>

                </el-table>
            </el-col>
        </el-row>
        <el-row>
            <el-col :span="24">
                <!-- 分页 -->
                <el-pagination @size-change="pageSizeChange"
                 @current-change="pageIndexChange"
                 :current-page="pageIndex" :page-sizes="[1,10, 20, 30, 40]"
                :page-size="pageSize" layout="total, sizes, prev, pager, next, jumper"
                :total="totalCount">
                </el-pagination>
            </el-col>
        </el-row>
    </div>
</template>

<script>

    export default {
        data() {
            return {
                pageSize:10, //定义一个分页组件中页容量，默认是10条
                pageIndex:1, //定义一个分页中的页索引，默认是第一页
                totalCount:0, //数据api返回的数据总行数
                // 搜索条件
                searchValue: '',
                list: []  // 用来存放商品数据数组，这个数组中的对象格式和/admin/goods/getlist响应回来的数据保持一致
            }
        },
        methods: {
            // 2.0 分页相关的事件
            // 获取当前的页索引 (pageindex)
            pageIndexChange(currentPage){
                console.log(currentPage);
                // 将用户选择的当前页索引赋值给pageindex
                this.pageIndex = currentPage;

                // 重新获取新数据
                this.getlist();
            },
            // 获取当前的页容量 （pagesize)
            pageSizeChange(size){
                console.log(size);
                this.pageSize = size;

                // 重新获取新数据
                this.getlist();
            },
            // 1.0 负责利用axios发出ajax请求：http://127.0.0.1:8899/admin/goods/getlist?pageIndex=1&pageSize=10&searchvalue=this.searchValue
            getlist() {
                //    1.0.1 定义url 路径
                var url = '/admin/goods/getlist?pageIndex='+this.pageIndex+'&pageSize='+this.pageSize+'&searchvalue=' + this.searchValue;

                // 1.0.2 请求(axios)
                this.$ajax.get(url).then(res => {
                    // 将商品数据数组赋值
                    this.list = res.data.message;

                    // 将商品的总数据条数赋值
                    this.totalCount = res.data.totalcount;
                });
            }
        },
        mounted() {
            // 当页面进入的时候就请求数据
            this.getlist();
        }
    }
</script>
<style scoped>

.unlinght{
     color:rgba(0,0,0,0.3);
 }

</style>