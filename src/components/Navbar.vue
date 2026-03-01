<template>
  <div class="oj-navbar">
    <div class="oj-navbar-menus">
      <img class="oj-navbar-logo" src="@/assets/logo.png" />
      <el-menu class="oj-navbar-menu" mode="horizontal">
        <el-menu-item>题库</el-menu-item>
        <el-menu-item>竞赛</el-menu-item>
      </el-menu>
    </div>
    <div class="oj-navbar-users">
      <img v-if="isLogin" class="oj-message" @click="goMessage" src="@/assets/message/message.png" />
      <el-dropdown v-if="isLogin">
        <div class="oj-navbar-name">
          <img class="oj-head-image" v-if="isLogin" :src="userInfo.headImage" />
          <span>{{ userInfo.nickName }}</span>
        </div>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item @click="goUserInfo">
              <div class="oj-navbar-item">
                <span>个人中心</span>
              </div>
            </el-dropdown-item>
            <el-dropdown-item @click="goMyExam">
              <div class="oj-navbar-item">
                <span>我的比赛</span>
              </div>
            </el-dropdown-item>
            <el-dropdown-item>
              <div class="oj-navbar-item">
                <span @click="handleLogout">退出登录</span>
              </div>
            </el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
      <span class="oj-navbar-login-btn" v-if="!isLogin" @click="goLogin">登录</span>
    </div>
  </div>
</template>

<script setup>
import { getUserInfoService, logoutService } from '@/apis/user';
import router from '@/router';
import { getToken, removeToken } from '@/utils/cookie';
import { reactive, ref } from 'vue';

const isLogin = ref(false)
const userInfo = reactive({
  nickName: '',
  headImage: ''
})

async function checkLogin() {
  if(getToken()) {
    //1.判断当前token是否过期
    //2.将当前用户的头像，昵称返回
    const userInfoRes = await getUserInfoService()
    Object.assign(userInfo, userInfoRes.data)
    isLogin.value = true;
  }
  
}
checkLogin()

function goLogin() {
  router.push("/c-oj/login")
}

async function handleLogout() {
    await ElMessageBox.confirm(
    '确认退出？',
    '温馨提示',
    {
      confirmButtonText: '确认',
      cancelButtonText: '取消',
      type: 'warning',
    }
  )
  await logoutService()
  removeToken()
  isLogin.value = false
}

</script>

<style scoped lang="scss">
.oj-navbar {
  width: 100%;
  height: 60px;
  background-color: #fff;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
  z-index: 100;
  position: relative;

  &-menus {
    display: flex;
    align-items: center;
    height: 100%;

    .oj-navbar-logo {
      height: 35px;
      margin-right: 30px;
      cursor: pointer;
    }

    .oj-navbar-menu {
      border-bottom: none !important;
      height: 100%;
      background: transparent;
      width: 300px;

      :deep(.el-menu-item) {
        font-size: 16px;
        height: 60px;
        line-height: 60px;
        
        &:hover {
          background-color: rgba(64, 158, 255, 0.1);
          color: #409eff;
        }
        
        &.is-active {
          color: #409eff;
          border-bottom: 2px solid #409eff;
        }
      }
    }
  }

  &-users {
    display: flex;
    align-items: center;
    
    .oj-message {
      width: 22px;
      height: 22px;
      margin-right: 25px;
      cursor: pointer;
      
      &:hover {
        opacity: 0.8;
      }
    }

    .oj-navbar-name {
      display: flex;
      align-items: center;
      cursor: pointer;
      margin-right: 10px;

      .oj-head-image {
        width: 36px;
        height: 36px;
        border-radius: 50%;
        margin-right: 10px;
        border: 1px solid #ebeef5;
      }

      span {
        font-size: 14px;
        color: #606266;
      }
      
      &:hover span {
        color: #409eff;
      }
    }

    .oj-navbar-login-btn {
      font-size: 14px;
      color: #606266;
      cursor: pointer;
      
      &:hover {
        color: #409eff;
      }
    }
  }
}

.oj-navbar-item {
  width: 100%;
  padding: 5px 0;
  text-align: center;
  
  span {
    display: block;
    width: 100%;
  }
}
</style>