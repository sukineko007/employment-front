<template>
    <div>
      <el-form :model="form" :rules="rules2" ref="form" label-position="left" label-width="0px" class="demo-ruleForm login-container">
        <h3 class="title">系统登录</h3>
        <el-form-item prop="account">
          <el-input type="text" v-model="form.username" auto-complete="off" placeholder="用户名(学生请使用学号登录)"></el-input>
        </el-form-item>
        <el-form-item prop="checkPass">
          <el-input type="password" v-model="form.password" auto-complete="off" placeholder="密码(学生默认密码123456)"></el-input>
        </el-form-item>
        <el-checkbox v-model="checked" checked class="remember">记住密码</el-checkbox>
        <h4>请选择用户类型：</h4>
        <el-select v-model="key" placeholder="请选择...">
          <el-option
            v-for="item in typeData"
            :key="item.id"
            :label="item.value"
            :value="item.id"
          ></el-option>
        </el-select>
        <el-form-item style="width:100%;">
          <el-button type="primary" style="width:100%;" @click="login" :loading="logining">登录</el-button>
          <!--<el-button @click.native.prevent="handleReset2">重置</el-button>-->
        </el-form-item>
      </el-form>
    </div>
</template>


<script>
  //   import { requestLogin } from '../api/api';
  import NProgress from 'nprogress'
  export default {
    data() {
      return {
        typeData:[
          {
            "id":1,
            "value":"管理员"
          },
          {
            "id":2,
            "value":"学生"
          },
          {
            "id":3,
            "value":"企业"
          }
        ],
        key:'',
        userData:{},
        userInfo:{},
        logining: false,
        form: {
          username: '',
          password: ''
        },
        rules2: {
          name: [
            { required: true, message: '请输入用户名', trigger: 'blur' },
            //{ validator: validaePass }
          ],
          password: [
            { required: true, message: '请输入密码', trigger: 'blur' },
            //{ validator: validaePass2 }
          ]
        },
        checked: true
      };
    },
    watch:{
      "this.form": function (newValue) {
        console.log(newValue);
      }
    },
    methods: {
      login(){  //登录
        if ( this.form.username == "") {
          this.$message.error('用户名不能为空！');
        }else if(this.form.password == ""){
          this.$message.error('密码不能为空！');
        }else
          var params = new URLSearchParams();
        params.append('type',this.key);
        params.append('username',this.form.username);
        params.append('password',this.form.password);
        if(this.key == 1) {
          this.axios.post('http://localhost:8700/login/login1', params)
            .then(res => {
              console.log(res.data);
              this.userData = res.data;
              this.logining = true;
              console.log(this.userData.type);
              if(this.userData.type == 1) {
                this.$message.success('登录成功！');
                NProgress.start();
                setTimeout(() => {
                  sessionStorage.clear()
                  sessionStorage.setItem('userInfo', JSON.stringify(res.data));
                  this.logining = true;
                  NProgress.done();
                  this.$router.push({path: '/Home'});
                }, 1000);
                console.log(this.userInfo);
              }
              else {
                this.$message.error('用户名或密码错误！');
                this.logining = false;
              }
            })
            .catch(res => {
              this.logining = false;
              console.log("error");
            })
        }else if(this.key == 2){
          this.axios.post('http://localhost:8700/login/login2', params)
            .then(res => {
              console.log(res.data);
              this.userData = res.data;
              this.logining = true;
              console.log(this.userData.type);
              if(this.userData.type == 2) {
                this.$message.success('登录成功！');
                NProgress.start();
                setTimeout(() => {
                  sessionStorage.clear();
                  sessionStorage.setItem('userInfo', JSON.stringify(res.data));
                  this.logining = true;
                  NProgress.done();
                  this.$router.push({path: '/Home'});
                }, 1000);
                console.log(this.userInfo);
              }
              else {
                this.$message.error('用户名或密码错误！');
                this.logining = false;
              }
            })
            .catch(res => {
              this.logining = false;
              console.log("error");
            })

        }else if(this.key == 3)
        {
          this.axios.post('http://localhost:8700/login/login3', params)
            .then(res => {
              console.log(res.data);
              this.userData = res.data;
              this.logining = true;
              console.log(this.userData.type);
              if(this.userData.type == 3) {
                this.$message.success('登录成功！');
                NProgress.start();
                setTimeout(() => {
                  sessionStorage.clear();
                  sessionStorage.setItem('userInfo', JSON.stringify(res.data));
                  this.logining = true;
                  NProgress.done();
                  this.$router.push({path: '/Home'});
                }, 1000);
                console.log(this.userInfo);
              }
            else {
                this.$message.error('用户名或密码错误！');
                this.logining = false;
              }
            })
            .catch(res => {
              this.logining = false;
              console.log("error");
            })
        }
        else
          this.$message.error('请选择用户类型！');
      }
    }
  }

</script>

<style lang="scss" scoped>
  .login-container {
    /*box-shadow: 0 0px 8px 0 rgba(0, 0, 0, 0.06), 0 1px 0px 0 rgba(0, 0, 0, 0.02);*/
    -webkit-border-radius: 5px;
    border-radius: 5px;
    -moz-border-radius: 5px;
    background-clip: padding-box;
    margin: 180px auto;
    width: 350px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
    .title {
      margin: 0px auto 40px auto;
      text-align: center;
      color: #505458;
    }
    .remember {
      margin: 0px 0px 35px 0px;
    }
  }
</style>
