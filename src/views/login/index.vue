<template>
  <div class="login">
    <div class="login-wrap">
      <div class="loginlogo">
        <img src="./logo_index.png" alt />
      </div>
      <!-- 登录页面 -->
      <el-form ref="form" :model="form" :rules="rules">
        <!-- prop属性需要设置到form-item上 -->
        <el-form-item prop="mobile">
          <el-input v-model="form.mobile" placeholder="请输入手机号"></el-input>
        </el-form-item>
        <el-form-item prop="code">
          <el-row>
            <el-col :span="14">
              <el-input v-model="form.code" placeholder="请输入验证码"></el-input>
            </el-col>
            <el-col :span="8" :offset="2">
              <!-- :disabled='isDisabled' -->
              <el-button class="codeBtn" @click="getCode" :disabled='!!timer'>{{ timer ? codeTime +'秒后获取验证码' : '获取验证码'}}</el-button>
            </el-col>
          </el-row>
        </el-form-item>
        <el-form-item prop="read">
          <el-checkbox v-model="form.read">
            我已阅读并同意
            <a href="#">用户协议</a>和
            <a href="#">隐私条款</a>
          </el-checkbox>
          <el-checkbox label="美食/餐厅线上活动" name="type"></el-checkbox>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" class="loginbtn" @click="login" :loading="loadingTag">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      form: {
        mobile: '',
        code: '',
        read: false
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
        ],
        read: [
          { required: true, message: '请先阅读用户协议', trigger: 'change' },
          // 限制结果必须为true，和上面一样，上面第一个数组为必填，第二个数组为限制长度(限制输入框内信息)
          // pattern:匹配结果只能为true的
          { pattern: /true/, message: '请先阅读用户协议', trigger: 'change' }
        ]
      },
      loadingTag: false,
      // 倒计时的时候
      codeTime: 10,
      // 设置一个定时器,
      timer: null
      // isDisabled:false,
    }
  },
  methods: {
    login () {
      //   得到el-form元素
      //   console.log(this.$refs)
      // validate: 验证当前表单元素中所有的规则，是否填写完毕 跟上面的required相关，而手机账号名和验证码是否正确是跟底下的接口相关的
      this.$refs['form'].validate(valid => {
        console.log(valid)
        //   valid为true时验证通过
        // valid为false时验证不通过
        if (valid) {
          //   console.log('验证通过')
          //  提交数据
          this.msgSubmit()
        } else {
          console.log('验证不通过')
        }
      })
    },
    msgSubmit () {
      this.loadingTag = true
      axios({
        url: 'http://ttapi.research.itcast.cn/mp/v1_0/authorizations',
        method: 'post',
        data: this.form
      })
        .then(res => {
          this.loadingTag = false
          this.$router.push('/')
          this.$message({
            message: '恭喜你，登录成功',
            type: 'success'
          })
        })
        .catch(err => {
          this.loadingTag = false
          console.log(err)
          this.$message.error('手机号或者密码输入错误')
        })
    },
    getCode () {
      // 获取dom表单
      this.$refs['form'].validateField('mobile', errMsg => {
        // errMsg是验证不通过时的提示信息
        console.log(errMsg)
        if (errMsg.trim().length > 0) {
          //  说明errMsg存在，表示验证不通过
          console.log('验证不通过')
          return
        }
        // this.isDisabled=true
        //  验证通过:开启定时器 倒计时
        console.log('这是验证通过的代码')
        this.timer = setInterval(() => {
          this.codeTime--
          // 当时间为0时  清除定时器
          if (this.codeTime <= 0) {
            clearTimeout(this.timer)
            //  重置倒计时
            this.codeTime = 10
            // 将定时器重置为null
            this.timer = null
            // this.isDisabled=false
          }
        }, 1000)
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
    .codeBtn {
      width: 100%;
      text-align: center;
      //    color:pink;
    }
  }
}
</style>
