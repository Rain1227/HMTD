<template>
  <div class="login">
    <!-- elementUI card卡片组件 -->
    <el-card class="login-card">
      <!-- 卡片内容 -->
      <div class="title">
        <img src="../../assets/img/logo_index.png" alt />
      </div>
      <!-- 表单 -->
      <!-- model属性，要绑定表单数据对象  rules表示绑定校验规则对象-->
      <!-- ref可以通过给标签定义一个ref属性, 然后通过this.$refs.属性 获取dom对象（ref获取了全部表单域el-form） -->
      <el-form ref="refForm" style="margin-top:40px" :model="loginForm" :rules="loginRules">
        <!-- 一个表单域就是一个from-item -->
        <el-form-item prop="mobile">
          <!-- 手机号 -->
          <el-input v-model="loginForm.mobile" placeholder="请输入手机号"></el-input>
        </el-form-item>

        <el-form-item prop="code">
          <!-- 验证码和发送验证码 -->
          <el-input v-model="loginForm.code" placeholder="请输入验证码" style="width:280px"></el-input>
          <el-button style="float:right" plain>发送验证码</el-button>
        </el-form-item>
        <!-- 勾选框 -->
        <el-form-item prop="checked">
          <el-checkbox v-model="loginForm.checked">我已阅读同意条款</el-checkbox>
        </el-form-item>
        <el-form-item>
          <!-- 点击登录，全局验证 -->
          <el-button type="primary" style="width:100%" @click="login">登录</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>
export default {
  data () {
    return {
      loginForm: {
        mobile: '', // 手机号
        code: '', // 验证码
        checked: false // 是否勾选协议
      },
      //   校验规则对象、
      loginRules: {
        //   key（要校验的字段名）：value（数组=>多条或者一条或者没有规则）
        mobile: [
          // required必填项,如果不填 就无法通过校验/如果为true,就表示该字段必填
          { required: true, message: '请输入您的手机号' },
          {
            pattern: /^1[3456789]\d{9}$/,
            message: '请输入正确的手机号'
          }
        ],
        code: [
          { required: true, message: '请输入您的验证码' },
          { pattern: /^\d{6}$/, message: '验证码为6位数字' }
        ],
        checked: [
          {
            //   validator是自定义校验函数
            validator: function (rlue, value, callback) {
              // rule代表当前规则没有用，value代表当前的值 =》表单字段checked的值
              // callback回调函数
              if (value) {
                //   如果是true就是选中了，通过校验
                callback()
              } else {
                //   没有选中，不通过校验
                callback(new Error('您必须勾选'))
              }
            }
          }
        ]
      }
    }
  },

  methods: {
    login () {
      // 校验整个表单的规则
      // validate 是一个方法 => 方法中传入的一个函数 两个校验参数  是否校验成功/未校验成功的字段
      this.$refs.refForm.validate(function (isok) {
        if (isok) {
          console.log('成功')
        }
      })
    }
  }
}
</script>

<style lang='less' scoped>
// 在单文件组件中,如果需要在style标签中使用 诸如less scss 需要在style标签上 给lang属性赋值
// scoped属性,让当前组件的样式只对当前自己的html起作用，不加scoped当前的style就会加到全局
.login {
  background: url("../../assets/img/login_bg.jpg");
  background-size: cover; //cover把背景图片扩展至足够大，以使背景图像完全覆盖在背景区域，背景图像的某些部分也许无法显示在背景定位区域中
  height: 100vh; //vh代表当前屏幕的百分比
  display: flex;
  justify-content: center;
  align-items: center;
  .title {
    text-align: center;
    img {
      height: 45px;
    }
  }

  .login-card {
    width: 450px;
    height: 360px;
  }
}
</style>
