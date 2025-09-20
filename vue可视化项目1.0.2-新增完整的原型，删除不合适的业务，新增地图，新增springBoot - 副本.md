# 

# 1 前端修改，使用已经搭好的前端框架

## 1.1 原型图:

```
├── 登录页（Login）【100%】
├── 仪表盘页（Dashboard）【99%】
├── 地图页（GISMapView）【98%】
│   ├── 图层切换器（LayerSwitcher）
│   ├── 点位弹窗（InfoPopup）
│   └── 地图搜索/定位
├── 设施管理页（AssetManagement）【97%】
│   ├── 列表/表格（List/Table）
│   ├── 设施详情（Detail/Drawer）
│   └── 新增/编辑/删除对话框
├── 工单管理页（WorkOrder）【96%】
│   ├── 工单列表
│   ├── 工单详情
│   └── 创建/编辑/指派对话框
├── 用户与角色页（UserRole）【92%】
│   ├── 用户管理
│   └── 角色管理
├── 设置页（Settings）【80%】
│   └── 系统配置、地图配置等
├── 日志/统计页（Logs/Stats）【75%】

```

```
[Login]
  ↓
[Dashboard]
  ↓             ↘
[GISMapView] <-> [AssetManagement] <-> [WorkOrder]
           ↘            ↘            ↘
          [Settings]   [UserRole]   [Logs/Stats]

```





------

### 1. 登录页

```plaintext
┌────────────────────────────┐ 
│      城市设备管理系统      │
│                            │
│   [ 用户名    ]            │
│   [ 密码      ] (可见)     │
│  [验证码 (可选)]            │
│ [ 登录 ]   [记住我]         │
│  忘记密码？ 注册账号        │ 👈 新增项
│  (错误提示区域)             │
└────────────────────────────┘

```

安装和使用mock插件

```
npm install vite-plugin-mock mockjs --save-dev

```



```
// vite.config.ts

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { viteMockServe } from 'vite-plugin-mock'

export default defineConfig(({ command }) => ({
  plugins: [
    vue(),

    // mock 插件：仅在本地开发时开启
    viteMockServe({
      mockPath: 'mock',        // mock 文件夹目录
      localEnabled: command === 'serve',  // 仅 dev 时开启
      supportTs: true          // 支持 TypeScript
    })
  ]
}))


mkdir mock
touch mock/auth.ts


// mock/auth.ts

import { MockMethod } from 'vite-plugin-mock'
import Mock from 'mockjs'

export default [
  {
    url: '/api/auth/login',   // 拦截的请求路径
    method: 'post',           // 请求方法
    response: ({ body }) => {
      const { username, password } = body

      // 模拟登录逻辑（你可以改成 token 校验、失败逻辑等）
      if (username === 'admin' && password === '123456') {
        return {
          code: 200,
          message: '登录成功',
          token: Mock.mock('@string(32)'), // 随机32位 token
          userInfo: {
            id: '1001',
            name: '管理员',
            role: 'admin',
            email: 'admin@example.com',
            avatarUrl: 'https://i.pravatar.cc/150?img=3'
          }
        }
      } else {
        return {
          code: 401,
          message: '用户名或密码错误'
        }
      }
    }
  }
] as MockMethod[]




```

登录业务

```
// services/auth.ts

import axios from 'axios'                              // 引入 axios 发送 HTTP 请求
import type { LoginPayload, LoginResponse } from '@/types/auth' // 引入登录参数与返回类型

/**
 * 登录函数
 * @param data - 登录请求参数对象
 * @returns Promise<LoginResponse> - 返回一个登录响应结果
 */
export async function login(data: LoginPayload): Promise<LoginResponse> {
  // 发送 POST 请求至 /api/auth/login 接口，提交登录数据
  const response = await axios.post<LoginResponse>('/api/auth/login', data)

  // 返回后端响应的数据内容（包含 token、用户信息等）
  return response.data
}

/**
 * 登出函数
 * @returns Promise<void> - 返回一个空 Promise 表示操作完成
 */
export async function logout(): Promise<void> {
  // 发送 POST 请求至 /api/auth/logout 接口，通知后端销毁会话
  await axios.post('/api/auth/logout')

  // 此处无需返回任何数据，仅用于告知操作完成
}


// store/modules/auth.ts

import { defineStore } from 'pinia'                          // 引入 Pinia 的 defineStore 创建 Store
import { ref } from 'vue'                                    // 使用 ref 创建响应式状态
import { login, logout } from '@/services/auth'              // 引入登录和登出接口
import type { LoginPayload, UserInfo } from '@/types/auth'   // 引入类型定义

// 定义并导出名为 useAuthStore 的 Store
export const useAuthStore = defineStore('auth', () => {
  // === 1. 状态区 ===
  const token = ref<string>('')                  // 登录成功后保存 token
  const userInfo = ref<UserInfo | null>(null)    // 保存当前登录用户信息（对象或 null）
  const isLoggedIn = ref<boolean>(false)         // 当前是否登录

  // === 2. 登录操作 ===
  const loginAction = async (payload: LoginPayload) => {
    // 调用登录接口，传入表单数据
    const res = await login(payload)

    // 判断是否登录成功（状态码为 200 且包含 token）
    if (res.code === 200 && res.token && res.userInfo) {
      token.value = res.token                    // 存储 token 到状态中
      userInfo.value = res.userInfo              // 存储用户信息
      isLoggedIn.value = true                    // 设置登录状态为 true

      // 可选：持久化 token 到 localStorage（如果勾选“记住我”）
      if (payload.rememberMe) {
        localStorage.setItem('auth_token', res.token)       // 存 token
        localStorage.setItem('auth_user', JSON.stringify(res.userInfo)) // 存用户
      }
    } else {
      throw new Error(res.message || '登录失败')  // 抛出错误供页面处理提示
    }
  }

  // === 3. 登出操作 ===
  const logoutAction = async () => {
    await logout()                               // 调用登出接口（后端处理销毁）
    clearState()                                 // 清空本地登录信息
  }

  // === 4. 清空本地状态（包括 localStorage）
  const clearState = () => {
    token.value = ''                             // 清空 token
    userInfo.value = null                        // 清空用户信息
    isLoggedIn.value = false                     // 设为未登录

    localStorage.removeItem('auth_token')        // 移除 token 存储
    localStorage.removeItem('auth_user')         // 移除用户存储
  }

  // === 5. 页面刷新后尝试恢复状态 ===
  const restoreAuthFromStorage = () => {
    const savedToken = localStorage.getItem('auth_token')        // 读取 token
    const savedUser = localStorage.getItem('auth_user')          // 读取用户信息

    if (savedToken && savedUser) {
      token.value = savedToken                                   // 恢复 token
      userInfo.value = JSON.parse(savedUser)                     // 恢复用户信息
      isLoggedIn.value = true                                    // 设为已登录
    }
  }

  // === 6. 返回对外暴露的状态和方法 ===
  return {
    token, userInfo, isLoggedIn,         // 状态
    loginAction, logoutAction,          // 操作
    clearState, restoreAuthFromStorage  // 工具函数
  }
})


<!-- views/role/LoginPage.vue -->
<template>
  <!-- 外层容器，垂直居中布局 -->
  <div class="min-h-screen flex items-center justify-center bg-gray-100">
    <!-- 登录框容器 -->
    <el-card class="w-[360px] shadow-md">
      <!-- 标题区域 -->
      <h2 class="text-xl font-bold mb-4 text-center">城市设备管理系统</h2>

      <!-- 表单区域 -->
      <el-form :model="form" :rules="rules" ref="formRef" label-position="top">
        <!-- 用户名输入框 -->
        <el-form-item label="用户名" prop="username">
          <el-input v-model="form.username" placeholder="请输入用户名" clearable />
        </el-form-item>

        <!-- 密码输入框 -->
        <el-form-item label="密码" prop="password">
          <el-input v-model="form.password" placeholder="请输入密码" show-password />
        </el-form-item>

        <!-- 记住我复选框 -->
        <el-form-item>
          <el-checkbox v-model="form.rememberMe">记住我</el-checkbox>
        </el-form-item>

        <!-- 登录按钮 -->
        <el-form-item>
          <el-button type="primary" class="w-full" @click="onSubmit">登录</el-button>
        </el-form-item>

        <!-- 底部链接：忘记密码 + 注册账号 -->
        <div class="flex justify-between text-sm text-gray-500">
          <a href="#" class="hover:underline">忘记密码？</a>
          <a href="#" class="hover:underline">注册账号</a>
        </div>
      </el-form>
    </el-card>
  </div>
</template>

<script setup lang="ts">
// 引入响应式 API
import { ref } from 'vue'

// 引入 Element Plus 表单相关类型
import type { FormInstance, FormRules } from 'element-plus'

// 引入 Pinia 登录模块
import { useAuthStore } from '@/store/modules/auth'

// 引入 Element Plus 消息提示
import { ElMessage } from 'element-plus'

// 1. 创建登录表单数据对象（响应式）
const form = ref({
  username: '',          // 用户名
  password: '',          // 密码
  rememberMe: false      // 是否记住登录状态
})

// 2. 定义表单校验规则
const rules: FormRules = {
  username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
  password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
}

// 3. 创建表单 ref 引用（用于手动校验）
const formRef = ref<FormInstance>()

// 4. 获取 Auth Store 实例
const authStore = useAuthStore()

// 5. 提交方法（点击登录时触发）
const onSubmit = () => {
  formRef.value?.validate(async valid => {
    if (!valid) return // 校验不通过不处理

    try {
      // 调用登录接口并自动更新状态
      await authStore.loginAction(form.value)

      // 登录成功提示
      ElMessage.success('登录成功')

      // 跳转主页（你可以替换成 dashboard）
      window.location.href = '/dashboard'
    } catch (err: any) {
      // 登录失败提示
      ElMessage.error(err.message || '登录失败')
    }
  })
}
</script>

<style scoped>
/* 可选：自定义样式 */
</style>



// router/modules/auth.ts

import { RouteRecordRaw } from 'vue-router'                   // 导入路由类型
// 导入视图组件（注意路径要根据你的实际路径）
const LoginPage = () => import('@/views/role/LoginPage.vue')  // 懒加载登录页面

// 定义登录相关的路由数组
const authRoutes: RouteRecordRaw[] = [
  {
    path: '/login',                 // 登录页路径
    name: 'Login',                  // 路由名
    component: LoginPage,           // 指定页面组件
    meta: {
      title: '登录',                // 元信息：页面标题
      requiresAuth: false           // 元信息：是否需要登录，登录页不需要
    }
  },
  // 你也可以在这里加注册、找回密码等路由
  // {
  //   path: '/register',
  //   name: 'Register',
  //   component: () => import('@/views/role/RegisterPage.vue'),
  //   meta: { title: '注册', requiresAuth: false }
  // }
]

export default authRoutes         // 导出给主路由入口注册



// router/index.ts

import { createRouter, createWebHistory } from 'vue-router'  // 引入 Vue Router 工具
import authRoutes from './modules/auth'                      // 导入登录相关路由
// import otherRoutes from './modules/other'                 // 可以继续导入其他业务路由

// 汇总所有路由（可以按需扩展多个模块）
const routes = [
  ...authRoutes,
  // ...otherRoutes
  // {
  //   path: '/',
  //   redirect: '/login',     // 可选：默认重定向到登录
  // }
]

// 创建路由实例
const router = createRouter({
  history: createWebHistory(),   // 路由模式：history（推荐，URL 美观无 #）
  routes                         // 路由数组
})

// 路由守卫：如需全局权限控制可在这里扩展
// router.beforeEach((to, from, next) => {
//   if (to.meta.requiresAuth && !isLoggedIn()) {
//     next('/login')
//   } else {
//     next()
//   }
// })

export default router


<!-- views/role/LoginPage.vue -->

<!-- views/role/LoginPage.vue -->
<template>
  <AuthLayout>
    <div class="login-page">
      <div class="login-card">
        <h1 class="login-title">登录</h1>
        <p class="login-desc">欢迎使用，请输入账号信息</p>
        <el-form
          :model="form"
          :rules="rules"
          ref="formRef"
          label-position="top"
          class="login-form"
        >
          <el-form-item label="用户名" prop="username">
            <el-input v-model="form.username" placeholder="请输入用户名" clearable />
          </el-form-item>
          <el-form-item label="密码" prop="password">
            <el-input v-model="form.password" placeholder="请输入密码" show-password />
          </el-form-item>
          <el-form-item>
            <el-checkbox v-model="form.rememberMe">记住我</el-checkbox>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" class="login-btn" @click="onSubmit" size="large">
              登录
            </el-button>
          </el-form-item>
        </el-form>
        <div class="login-footer">
          <a href="#" class="footer-link">忘记密码？</a>
          <span class="footer-sep">|</span>
          <a href="#" class="footer-link">注册账号</a>
        </div>
      </div>
    </div>
  </AuthLayout>
</template>

<script setup lang="ts">
import AuthLayout from '@/layouts/AuthLayout.vue'           // 引入布局外壳
import { ref } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'
import { useAuthStore } from '@/store/modules/auth'
import { ElMessage } from 'element-plus'

// 1. 登录表单数据
const form = ref({
  username: '',
  password: '',
  rememberMe: false
})

// 2. 校验规则
const rules: FormRules = {
  username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
  password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
}

// 3. 引用表单
const formRef = ref<FormInstance>()

// 4. 获取 pinia 登录模块
const authStore = useAuthStore()

// 5. 登录方法
const onSubmit = () => {
  formRef.value?.validate(async valid => {
    if (!valid) return
    try {
      await authStore.loginAction(form.value)
      ElMessage.success('登录成功')
      window.location.href = '/dashboard' // 登录成功跳转
    } catch (err: any) {
      ElMessage.error(err.message || '登录失败')
    }
  })
}
</script>


<style scoped lang="scss">
.login-page {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: $primary-bg; // 用变量
}
.login-card {
  min-width: 340px;
  width: 100%;
  max-width: 400px;
  background: $card-bg;
  box-shadow: $shadow-base;
  border-radius: $border-radius-base;
  padding: 36px 28px 24px 28px;
  display: flex;
  flex-direction: column;
  align-items: stretch;
  color: $text-color;
}
.login-title {
  font-size: $font-size-title;
  font-weight: 700;
  margin-bottom: 0.6em;
  text-align: center;
  letter-spacing: .02em;
  color: $primary-color;
}
.login-desc {
  color: $text-color-light;
  font-size: 1.05rem;
  margin-bottom: 1.6em;
  text-align: center;
}
.login-form {
  margin-bottom: 1.2em;
}
.login-btn {
  width: 100%;
  font-size: 1.1rem;
  border-radius: 12px;
  padding: 12px 0;
  background: $primary-color !important;
  border: none;
  color: #fff !important;
  transition: background .2s;
  &:hover, &:focus {
    background: $primary-color-dark !important;
  }
}
.login-footer {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0.7em;
  margin-top: 1.2em;
  font-size: 0.97em;
  color: $text-color-light;
  user-select: none;
}
.footer-link {
  color: $text-color-light;
  text-decoration: none;
  transition: color .2s;
  &:hover {
    color: $primary-color-dark;
    text-decoration: underline;
  }
}
.footer-sep {
  color: #cbd5e1;
  font-size: 1em;
}
@media (max-width: 480px) {
  .login-card { min-width: unset; max-width: 98vw; padding: 18px 8vw 16px 8vw; }
  .login-title { font-size: 1.3rem; }
  .login-desc { font-size: 1rem; }
}
</style>




<!-- layouts/AuthLayout.vue -->
<template>
  <div class="auth-center-bg">
    <div class="auth-center-content">
      <slot />
    </div>
  </div>
</template>

<script setup lang="ts">
// 无脚本
</script>

<style scoped lang="scss">
/* 可用全局变量，也可直接写浅色 */
@use "@/styles/variables" as *;

.auth-center-bg {
  min-height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  background: $primary-bg; // 用你的全局背景变量
  overflow: hidden;
}

.auth-center-content {
  /* 最大宽度适配所有屏幕，PC不会太窄 */
  width: 100%;
  max-width: 420px;
  padding: 0 1rem;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>

```











------

### 2. 仪表盘

```plaintext
┌───────────────────────────────────────────────┐
│ 顶部导航 | 统计卡片 | 快捷入口                │
│ ┌───────┬───────┬───────┐                    │
│ │ 设备总数│在线率   │工单总数│ ...            │
│ └───────┴───────┴───────┘                    │
│                                               │
│ ┌─────────────┬──────────────┐                │
│ │ 设备分布小地图 │ 最近工单列表 │             │
│ └─────────────┴──────────────┘                │
└───────────────────────────────────────────────┘
```

![Generated image](https://sdmntprsouthcentralus.oaiusercontent.com/files/00000000-c5e0-61f7-a4e0-9a25c2dd0dd3/raw?se=2025-05-30T12%3A27%3A19Z&sp=r&sv=2024-08-04&sr=b&scid=677f6c6c-cb52-5868-a245-26e9a54ede85&skoid=24a7dec3-38fc-4904-b888-8abe0855c442&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-05-30T00%3A21%3A44Z&ske=2025-05-31T00%3A21%3A44Z&sks=b&skv=2024-08-04&sig=vbKDFzdnYZaq7XtGJR4zWn3dj0DijcJ3E7xCEWWn67M%3D)

------

### 3. 地图页（GISMapView）

```plaintext
┌──────────────────────────────────────────────────────────────┐
│ 顶部导航栏                  [地图筛选/搜索/图层切换]         │
│ ┌────────────────────────────────────────────────────────┐   │
│ │                    地 图 区 域                        │   │
│ │   (设备点位/颜色/状态，点击弹出信息，右侧抽屉详情)     │   │
│ │                                                      │   │
│ └────────────────────────────────────────────────────────┘   │
│ [左侧为菜单栏，右侧为详情抽屉/工单弹窗]                      │
└──────────────────────────────────────────────────────────────┘
```

![Generated image](https://sdmntprsouthcentralus.oaiusercontent.com/files/00000000-5494-61f7-9dfa-c007b9001fc8/raw?se=2025-05-30T12%3A32%3A35Z&sp=r&sv=2024-08-04&sr=b&scid=626509bc-9aa3-5396-9a6e-9e901fcc8a99&skoid=24a7dec3-38fc-4904-b888-8abe0855c442&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-05-30T00%3A21%3A40Z&ske=2025-05-31T00%3A21%3A40Z&sks=b&skv=2024-08-04&sig=bqqn0IV9W3C9R%2BZSdEFmD33CxM543XEe3wc88fuu7dE%3D)

------

### 4. 设施管理页

```plaintext
┌────────────────────────────────────────────────────────┐
│ 顶部导航栏 | 搜索 | 筛选 | 新增设备                    │
│ ┌───────────────────────────────────────────────┐      │
│ │ 设备列表（表格，支持分页/筛选/批量操作/导出）  │      │
│ │ [名称][类型][状态][位置][操作] ...            │      │
│ │ [编辑] [删除] [地图定位]                      │      │
│ └───────────────────────────────────────────────┘      │
│ 新增/编辑为弹窗或侧边栏抽屉                           │
└────────────────────────────────────────────────────────┘
```

![Generated image](https://sdmntpritalynorth.oaiusercontent.com/files/00000000-0c4c-6246-b36b-114b2e816f65/raw?se=2025-05-30T12%3A13%3A46Z&sp=r&sv=2024-08-04&sr=b&scid=6f6a182c-8919-58bc-b01d-d98e9ca0beb6&skoid=b32d65cd-c8f1-46fb-90df-c208671889d4&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-05-30T05%3A59%3A48Z&ske=2025-05-31T05%3A59%3A48Z&sks=b&skv=2024-08-04&sig=8f9H%2BXq1VKZREsfZSZS%2BP19aBBKibEFLqW/h4wxK9G0%3D)

------

### 5. 工单管理页

```plaintext
┌────────────────────────────────────────────────────────┐
│ 顶部导航栏 | 搜索 | 筛选 | 创建工单                    │
│ ┌───────────────────────────────────────────────┐      │
│ │ 工单列表（表格，支持分页/筛选/导出）         │      │
│ │ [编号][设备][状态][处理人][操作] ...          │      │
│ │ [详情][指派][归档]                            │      │
│ └───────────────────────────────────────────────┘      │
│ 详情/指派等均为弹窗或侧边栏抽屉                   │
└────────────────────────────────────────────────────────┘
```

![Generated image](https://sdmntprnortheu.oaiusercontent.com/files/00000000-0864-61f4-87f0-cd9e253cc520/raw?se=2025-05-30T12%3A15%3A52Z&sp=r&sv=2024-08-04&sr=b&scid=80cf17f1-65a6-5e8c-bcc2-d457ad636e07&skoid=b32d65cd-c8f1-46fb-90df-c208671889d4&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-05-30T08%3A15%3A19Z&ske=2025-05-31T08%3A15%3A19Z&sks=b&skv=2024-08-04&sig=VqD3tQXGdt2HEN2J2cri3DTxEhylLWeq1ee6PBwuqF4%3D)

------

### 6. 用户与角色管理页

```plaintext
┌─────────────────────────────────────────────┐
│ 顶部导航栏 | 用户管理 | 角色管理            │
│ ┌───────────────┬──────────────────────┐   │
│ │ 用户表格      │ 角色/权限树           │   │
│ │ [用户名][角色][状态][操作] ...        │   │
│ │ [编辑][重置密码][禁用]                │   │
│ └───────────────┴──────────────────────┘   │
└─────────────────────────────────────────────┘
```

![Generated image](https://sdmntpritalynorth.oaiusercontent.com/files/00000000-094c-6246-b9fe-c95cd632c925/raw?se=2025-05-30T12%3A20%3A56Z&sp=r&sv=2024-08-04&sr=b&scid=76d4a448-285b-552d-8c41-608a38b84921&skoid=b32d65cd-c8f1-46fb-90df-c208671889d4&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-05-30T04%3A53%3A54Z&ske=2025-05-31T04%3A53%3A54Z&sks=b&skv=2024-08-04&sig=9iHElp600g1iXXJ%2Bjmlc2PsnKunh%2B4oxIjwTyEGVZdg%3D)

![Generated image](https://sdmntprnortheu.oaiusercontent.com/files/00000000-51d0-61f4-a581-151b75c5d58b/raw?se=2025-05-30T12%3A23%3A06Z&sp=r&sv=2024-08-04&sr=b&scid=c7e44dad-5ce2-5911-aa80-127dd414e213&skoid=b32d65cd-c8f1-46fb-90df-c208671889d4&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-05-29T23%3A47%3A40Z&ske=2025-05-30T23%3A47%3A40Z&sks=b&skv=2024-08-04&sig=DV0P3oOmGOZmtKxdO3vnRg8uXeAmhUF6m0l8RM8TqhY%3D)



------

### 7. 设置页

```plaintext
┌─────────────────────────────────────────────┐
│ 顶部导航栏 | 地图配置 | 系统参数            │
│ ┌───────────────┬──────────────────────┐   │
│ │ 地图源选择    │ 坐标系                │   │
│ │ 下拉菜单      │ 输入/选择             │   │
│ │ 其它系统参数  │ ...                  │   │
│ └───────────────┴──────────────────────┘   │
└─────────────────────────────────────────────┘
```

![Generated image](https://sdmntprnortheu.oaiusercontent.com/files/00000000-20c8-61f4-8816-845348078a83/raw?se=2025-05-30T12%3A25%3A46Z&sp=r&sv=2024-08-04&sr=b&scid=7010c68a-7bcd-50c8-aa04-c4065c092050&skoid=b32d65cd-c8f1-46fb-90df-c208671889d4&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-05-30T05%3A35%3A03Z&ske=2025-05-31T05%3A35%3A03Z&sks=b&skv=2024-08-04&sig=gvuFMUjg1fg7ynKUSdWy1J5m0wP0JzqIyLOhvK2Trb8%3D)

------

### 8. 日志/统计页

```plaintext
┌─────────────────────────────────────────────┐
│ 顶部导航栏 | 日志列表 | 统计报表             │
│ ┌────────────────────┬───────────────────┐  │
│ │ 操作日志表格        │ 统计图表区         │  │
│ │ [时间][用户][操作][详情] ...            │  │
│ │ [导出] [筛选]                          │  │
│ └────────────────────┴───────────────────┘  │
└─────────────────────────────────────────────┘
```

![Generated image](https://sdmntpritalynorth.oaiusercontent.com/files/00000000-0de0-6246-98f5-6123af585da4/raw?se=2025-05-30T13%3A26%3A27Z&sp=r&sv=2024-08-04&sr=b&scid=8fb75aa2-8926-5d81-9519-fc67c44dee80&skoid=b32d65cd-c8f1-46fb-90df-c208671889d4&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-05-30T12%3A19%3A57Z&ske=2025-05-31T12%3A19%3A57Z&sks=b&skv=2024-08-04&sig=WHkplQPBUwtBSRRtgBI/73fqvieNfqTwTqHh4t%2Bj3Oo%3D)

------







## 2 后端修改，使用已经搭好的后端框架



### 1. 登录页

```
-- 1. 创建数据库（可根据你的项目名自定义）
CREATE DATABASE IF NOT EXISTS `project_auth`
  DEFAULT CHARACTER SET utf8mb4
  DEFAULT COLLATE utf8mb4_general_ci;

USE `project_auth`;

-- 2. 用户主表
CREATE TABLE `user` (
  `id`           BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY COMMENT '用户ID',
  `username`     VARCHAR(64) NOT NULL UNIQUE COMMENT '用户名/工号/邮箱，唯一',
  `password`     VARCHAR(255) NOT NULL COMMENT '密码（强制加密存储）',
  `name`         VARCHAR(64) NOT NULL COMMENT '昵称/姓名',
  `email`        VARCHAR(128) UNIQUE COMMENT '邮箱（可用于找回密码/通知）',
  `role`         VARCHAR(32) DEFAULT 'user' COMMENT '角色代码（admin/user等）',
  `avatar_url`   VARCHAR(255) DEFAULT NULL COMMENT '头像URL',
  `is_active`    TINYINT(1) DEFAULT 1 COMMENT '是否激活 1=激活,0=禁用',
  `created_at`   DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `updated_at`   DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '最近更新时间'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='用户主表';

-- 3. 角色表
CREATE TABLE `role` (
  `id`        BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY COMMENT '角色ID',
  `code`      VARCHAR(32) NOT NULL UNIQUE COMMENT '角色代码（如admin、user、manager）',
  `name`      VARCHAR(64) NOT NULL COMMENT '角色中文名称',
  `desc`      VARCHAR(255) DEFAULT NULL COMMENT '角色描述',
  `created_at`   DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `updated_at`   DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='角色表';

-- 4. 用户-角色关联表
CREATE TABLE `user_role` (
  `user_id` BIGINT UNSIGNED NOT NULL COMMENT '用户ID',
  `role_id` BIGINT UNSIGNED NOT NULL COMMENT '角色ID',
  `created_at`   DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '绑定时间',
  PRIMARY KEY (`user_id`, `role_id`),
  CONSTRAINT `fk_userrole_user` FOREIGN KEY (`user_id`) REFERENCES `user`(`id`) ON DELETE CASCADE,
  CONSTRAINT `fk_userrole_role` FOREIGN KEY (`role_id`) REFERENCES `role`(`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='用户-角色关联表';

-- 5. 登录历史表
CREATE TABLE `login_log` (
  `id`         BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY COMMENT '日志ID',
  `user_id`    BIGINT UNSIGNED NOT NULL COMMENT '用户ID',
  `login_time` DATETIME NOT NULL COMMENT '登录时间',
  `ip`         VARCHAR(45) DEFAULT NULL COMMENT '登录IP',
  `ua`         VARCHAR(255) DEFAULT NULL COMMENT 'User Agent',
  `status`     TINYINT(1) DEFAULT 1 COMMENT '1=成功，0=失败',
  CONSTRAINT `fk_loginlog_user` FOREIGN KEY (`user_id`) REFERENCES `user`(`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='登录历史表';

-- 6. Token管理表
CREATE TABLE `token` (
  `id`         BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY COMMENT 'Token记录ID',
  `user_id`    BIGINT UNSIGNED NOT NULL COMMENT '用户ID',
  `token`      VARCHAR(255) NOT NULL COMMENT 'Token值',
  `expire_at`  DATETIME NOT NULL COMMENT 'Token过期时间',
  `revoked`    TINYINT(1) DEFAULT 0 COMMENT '0=有效，1=已撤销',
  `created_at` DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  CONSTRAINT `fk_token_user` FOREIGN KEY (`user_id`) REFERENCES `user`(`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='Token管理表';


```



```

```

