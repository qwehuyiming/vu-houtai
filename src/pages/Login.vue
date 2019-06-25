<template>
  <!-- el-form是表单组件 -->
  <!-- ref 可以操作dom -->
  <!-- model 当前表单数据 -->
  <!-- rules 表单的校验规则 -->
  <div class="form-wrapper">
    <el-form ref="form" :model="form" :rules="rules" label-width="80px" class="form">

      <el-form-item label="账号" prop="username">
        <el-input v-model="form.username"></el-input>
      </el-form-item>

      <el-form-item label="密码" prop="password">
        <el-input type="password" v-model="form.password"></el-input>
      </el-form-item>

      <el-form-item>
        <!-- type按钮的颜色 -->
        <el-button type="primary" @click="onSubmit">登录</el-button>
        <el-button>取消</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        // 表单的数据
        form: {
          username: "",
          password: ""
        },
        // 表单规则
        rules: {
          username: [
            // required 必须要输入, message提示的信息，trigger blur失去焦点时候触发
            { required: true, message: '请输入用户名', trigger: 'blur' },
          ],
          password: [
            { required: true, message: '请输入密码', trigger: 'blur' },
          ],
        }
      }
    },
    methods: {
      // 登录事件，提交账号密码到服务器
      onSubmit() {

        // 提交到后台的数据
        const data = {
          uname: this.form.username,
          upwd: this.form.password
        }

        // 如果表单的验证不通过，不提交表单
        this.$refs.form.validate((valid) => {
          
          // 验证通过
          if (valid) {
            
            // 调用axios
            this.$axios({
              // 请求的地址
              url: "http://localhost:8899/admin/account/login",
              // 请求方式
              method: "POST",
              // 数据
              data,
              // 处理session跨域
              withCredentials: true
            }).then(res => {
              // 解构并且赋值
              const {message, status} = res.data;

              // $store下的方法commit设置仓库的数据
              // commit方法调用仓库中mutations的方法，第一个参数是方法名
              // 第二个参数就是传递的数据对象
              this.$store.commit("setUser", message);

              // 登录成功
              if(status === 0){
                this.$router.push("/");
              }
              
              // 登录失败
              if(status === 1){
                this.$message.error(message);
              }
            })
          }
        });
      }
    }
  }
</script>

<style scoped>
.form-wrapper{
  width:100%;
  position:absolute;
  top:0; 
  bottom:0;
}

.form{
  width:500px;
  position: absolute;
  left:50%;
  margin-left:-250px;
  top:50%;
  margin-top:-95px;
}
</style>
