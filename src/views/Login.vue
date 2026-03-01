<template>
  <div class="login-page">
    <div class="orange"></div>
    <div class="blue"></div>
    <div class="blue small"></div>
    <div class="login-box">
      <div class="logo-box">
        <img src="@/assets/logo.png">
        <div>
          <div class="sys-name">比特OJ</div>
          <div class="sys-sub-name">帮助100万大学生就业</div>
        </div>
      </div>
      <div class="form-box-title">
        <span>验证码登录</span>
      </div>
      <div class="form-box">
        <div class="form-item">
          <img src="@/assets/images/shouji.png">
          <el-input v-model="mobileForm.phone" type="text" placeholder="请输入手机号" />
        </div>
        <div class="form-item">
          <img src="@/assets/images/yanzhengma.png">
          <el-input v-model="mobileForm.code" type="text" placeholder="请输入验证码" />
          <div class="code-btn-box" @click="getCode">
            <span>{{ txt }}</span>
          </div>
        </div>
      </div>
      <div class="submit-box" @click="loginFun">
        登录/注册
      </div>
    </div>
  </div>
</template>
<script setup>
import { codeLoginService, sendCodeService } from '@/apis/user';
import router from '@/router';
import { reactive, ref } from 'vue'
import { setToken } from '@/utils/cookie';

let mobileForm = reactive({
  phone: '',
  code: ''
})

let txt = ref('获取验证码')
let timer = null;

async function getCode() {
  await sendCodeService(mobileForm)
  txt.value = '59s'
  let num = 59
  timer = setInterval(() => {
    num--
    if( num < 1 ) {
      txt.value = '重新获取验证码'
      clearInterval(timer)
    } else {
      txt.value = num + 's'
    }
  }, 1000)
}

async function loginFun() {
  const loginRef = await codeLoginService(mobileForm)
  setToken(loginRef.data)
  router.push("/c-oj/home")
}
</script>

<style scoped lang="scss">
.login-page {
  position: relative;
  width: 100vw;
  height: 100vh;
  background: #f0f2f5;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;

  .orange {
    position: absolute;
    width: 300px;
    height: 300px;
    background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 100%);
    border-radius: 50%;
    top: -50px;
    left: -50px;
    opacity: 0.5;
    filter: blur(50px);
  }

  .blue {
    position: absolute;
    width: 400px;
    height: 400px;
    background: linear-gradient(135deg, #a18cd1 0%, #fbc2eb 100%);
    border-radius: 50%;
    bottom: -100px;
    right: -100px;
    opacity: 0.5;
    filter: blur(60px);

    &.small {
      width: 200px;
      height: 200px;
      background: linear-gradient(135deg, #84fab0 0%, #8fd3f4 100%);
      top: 20%;
      right: 20%;
      bottom: auto;
      filter: blur(40px);
    }
  }

  .login-box {
    position: relative;
    z-index: 10;
    width: 400px;
    background: #fff;
    padding: 40px;
    border-radius: 12px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);

    .logo-box {
      display: flex;
      align-items: center;
      margin-bottom: 30px;

      img {
        width: 48px;
        height: 48px;
        margin-right: 12px;
      }

      .sys-name {
        font-size: 24px;
        font-weight: 600;
        color: #333;
        line-height: 1.2;
      }

      .sys-sub-name {
        font-size: 12px;
        color: #999;
        margin-top: 4px;
      }
    }

    .form-box-title {
      margin-bottom: 24px;
      
      span {
        font-size: 18px;
        font-weight: 500;
        color: #333;
        position: relative;
        
        &::after {
          content: '';
          display: block;
          width: 30px;
          height: 3px;
          background: #409eff;
          margin-top: 8px;
          border-radius: 2px;
        }
      }
    }

    .form-box {
      .form-item {
        display: flex;
        align-items: center;
        background: #f5f7fa;
        border-radius: 8px;
        padding: 0 16px;
        margin-bottom: 20px;
        height: 48px;
        transition: all 0.3s;

        &:focus-within {
          background: #fff;
          box-shadow: 0 0 0 1px #409eff;
        }

        img {
          width: 20px;
          height: 20px;
          margin-right: 12px;
          opacity: 0.6;
        }

        :deep(.el-input) {
          flex: 1;
          --el-input-border-color: transparent;
          --el-input-hover-border-color: transparent;
          --el-input-focus-border-color: transparent;
          --el-input-bg-color: transparent;
          
          .el-input__wrapper {
            box-shadow: none !important;
            padding: 0;
            background: transparent;
          }
        }

        .code-btn-box {
          margin-left: 12px;
          cursor: pointer;
          font-size: 14px;
          color: #409eff;
          white-space: nowrap;
          user-select: none;

          &:hover {
            color: #66b1ff;
          }
        }
      }
    }

    .submit-box {
      width: 100%;
      height: 48px;
      background: linear-gradient(90deg, #409eff, #36cfc9);
      border-radius: 24px;
      color: #fff;
      font-size: 16px;
      font-weight: 500;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: opacity 0.3s;
      margin-top: 30px;
      user-select: none;

      &:hover {
        opacity: 0.9;
        box-shadow: 0 4px 12px rgba(64, 158, 255, 0.4);
      }
      
      &:active {
        opacity: 1;
      }
    }
  }
}
</style>