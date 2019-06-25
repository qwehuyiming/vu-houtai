<template>
    <div>
        <!-- data: 组件接收的数据 -->
        <!-- show-checkbox: 是否显示选择框 -->
        <!-- node-key: 类似for循环的:key -->
        <!-- default-expand-all: 默认展开全部 -->
        <!-- expand-on-click-node：是否在点击节点的时候展开或者收缩节点 -->
        <el-tree
        :data="data"
        show-checkbox
        node-key="id"
        default-expand-all
        :expand-on-click-node="false">
            <!-- { node, data } 相当于 const { node, data } = scope -->
            <!-- scope.data:代表每一行的数据对象
            scope.node代表每一行的数据对象,只包含id和label -->
            <span class="custom-tree-node" slot-scope="{ node, data }">
                <span>{{ data.title }}</span>
                <span>
                    <input 
                    :value="data.sort_id"
                    @blur="handleBlur($event, data)"/>
                </span>
                <span>
                    <el-button
                        type="text"
                        size="mini">
                       编辑
                    </el-button>
                </span>
            </span>
        </el-tree>
    </div>
</template>

<script>
export default {
    data() {
      
      return {
        data: [],
      }
    },

    // watch: {
    //     data: {
    //         deep: true,
    //         handler(){
    //             console.log("data发生了变化")
    //         }
    //     }
    // },

    methods: {
        handleBlur(event, data){

            // 如果先后修改的值如果是相等，不执行下面的代码
            if(data.sort_id == event.target.value){
                return;
            }
            
            // 通过弹窗询问是否编辑
            let res = window.confirm("是否确认编辑？");

            // 如果确定编辑
            if(res){
                this.$axios({
                    url: "http://localhost:8899/admin/category/edit/" + data.category_id,
                    method: "POST",
                    data: {
                        ...data,
                        sort_id: event.target.value
                    },
                    // 处理session跨域
                    withCredentials: true
                }).then(res => {
                    this.getList();
                    this.$message.success("修改成功");
                })
            }
        },

        // 请求列表的数据
        getList(){
            // 请求分类数据
            this.$axios({
                url: "http://localhost:8899/admin/category/getlist/goods",
                method: 'GET'
            }).then(res => {
                const {status, message} = res.data;

                if(status === 0){
                    //  把列表的数据赋值给data，渲染到树形控件
                    this.data = message;
                }
            })
        }
    },

    mounted(){
        this.getList();
    }
}
</script>

<style>
.custom-tree-node {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 14px;
    padding-right: 8px;
  }

  .custom-tree-node input{
      width:80px;
      height:20px!important;
  }
</style>
