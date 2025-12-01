<script setup>
// const goList = () => {
//   this.$router.push('/list')
// }
import { useRouter, useRoute } from 'vue-router'
const router = useRouter() //路由对象
const route = useRoute() //路由参数

const goList = () => {
  router.push('/list')
  console.log(router, route)
}
</script>

<template>
  <div>
    我是App
    <el-button @click="$router.push('/home')">跳首页</el-button>
    <el-button @click="goList">跳列表页</el-button>
    <el-button type="primary">Primary</el-button>
    <el-button type="success">Success</el-button>
    <p>{{ token }}</p>
    <el-button>登录</el-button>
    <el-button></el-button>
  </div>
</template>

<style scoped></style>
