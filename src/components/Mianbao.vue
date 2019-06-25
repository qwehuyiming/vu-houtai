<template>
    <div class="mianbao">
        <el-breadcrumb separator="/">
            <el-breadcrumb-item v-for="(item, index) in info" :key="index">
                {{item}}
            </el-breadcrumb-item>
        </el-breadcrumb>
    </div>
</template>

<script>
export default {
    data(){
        return {
            info: []
        }
    },

    // 获取meta信息
    watch: {
        $route(){
            this.getRoute();
        }
    },

    methods:{
        // 保存meta信息
        getRoute(){
            // 获取到matched的每一项的meta信息
            const arr = this.$route.matched;
            let info = [];

            arr.forEach(v => {
                info.push(v.meta)
            });

            this.info = info;
        }
    },

    mounted(){
        // 组件加载完成后获取meta信息
        this.getRoute()
    }
}
</script>

<style scoped>
.mianbao{
    padding-bottom: 10px;
    border-bottom: 1px #ddd solid;
    margin-bottom:20px;
}
</style>
