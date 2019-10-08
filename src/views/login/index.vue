<template>
  <div class="login">
    <div class="login-wrap">
      <div class="loginlogo">
        <img src="./logo_index.png" alt />
      </div>
      <!-- 登录页面 -->
      <el-form ref="form" :model="form" :rules='rules'>
          <!-- prop属性需要设置到form-item上 -->
        <el-form-item prop='mobile' >
          <el-input v-model="form.mobile" placeholder="请输入手机号"></el-input>
        </el-form-item>
        <el-form-item prop='code'>
          <el-row>
            <el-col :span="14" >
              <el-input v-model="form.code" placeholder="请输入验证码"></el-input>
            </el-col>
            <el-col :span="8" :offset='2' >
              <el-button class='codeBtn'>请输入验证码</el-button>
            </el-col>
          </el-row>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" class="loginbtn" @click='login'>登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      form: {
        mobile: '',
        code: ''
      },
      rules: {
        mobile: [
          // 必填
          { required: true, message: '请输入手机号码', trigger: 'blur' },
          { min: 11, max: 11, message: '长度必须为11', trigger: 'blur' }
        ],
        code: [
          { required: true, message: '请输入验证码', trigger: 'blur' },
          { min: 6, max: 6, message: '长度必须为6', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    login () {
      //   得到el-form元素
    //   console.log(this.$refs)
    // validate: 验证当前表单元素中所有的规则
      this.$refs['form'].validate(valid => {
        console.log(valid)
        //   valid为true时验证通过
        // valid为false时验证不通过
        if (valid) {
          console.log('验证通过')
        } else {
          console.log('验证不通过')
        }
      })
    }
  }
}
</script>

<style lang='less' scoped>
.login {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ccc;
  .login-wrap {
    background-color: #fff;
    padding: 30px;
    min-width: 300px;
    .loginlogo {
      text-align: center;
      img {
        width: 150px;
        margin-bottom: 20px;
      }
    }
    .loginbtn {
      width: 100%;
    }
    .codeBtn{
       width: 100%;
       text-align:center;
    //    color:pink;
    }
  }
}
</style>
