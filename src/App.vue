<script setup>
// const goList = () => {
//   this.$router.push('/list')
// }
import { useRouter, useRoute } from 'vue-router'
import { useUserStore, useCountStore } from '@/stores'
const router = useRouter() //路由对象
const route = useRoute() //路由参数

const goList = () => {
  router.push('/list')
  console.log(router, route)
}
const userStore = useUserStore()
const countStore = useCountStore()
</script>

<template>
  <div>
    <hr />
    <router-view></router-view>
    <hr />
    <hr />
    <hr />

    我是App
    <el-button @click="$router.push('/home')">跳首页</el-button>
    <el-button @click="goList">跳列表页</el-button>
    <el-button type="primary">Primary</el-button>
    <p></p>
    <el-button type="success">Success</el-button>
    <p>{{ userStore.token }}</p>
    <el-button @click="userStore.setToken('我调用了setToken函数哟')"
      >登录</el-button
    >
    <el-button @click="userStore.removeToken()">退出</el-button>
    <p>count: {{ countStore.count }}</p>
    <el-button @click="countStore.add(2)">count加2</el-button>
  </div>
</template>

<style scoped></style>
