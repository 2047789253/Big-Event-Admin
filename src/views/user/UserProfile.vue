<script setup>
import PageContainer from '@/components/PageContainer.vue'
import { ref } from 'vue'
import { useUserStore } from '@/stores'

const userStore = useUserStore()
// 表单数据
const formRef = ref()
const formModel = ref({
  username: userStore.user.username,
  nickname: userStore.user.nickname,
  email: userStore.user.email
})
// 表单校验规则
const rules = {
  nickname: [
    { required: true, message: '请输入用户昵称', trigger: 'blur' },
    {
      pattern: /^\S{2,10}$/,
      message: '昵称必须是2-10位的非空字符',
      trigger: 'blur'
    }
  ],
  email: [
    { required: true, message: '请输入用户邮箱', trigger: 'blur' },
    {
      type: 'email',
      message: '请输入正确的邮箱格式',
      trigger: ['blur', 'change']
    }
  ]
}
// 提交修改
const submitForm = async () => {
  await formRef.value.validate()
  // TODO: 调用接口更新用户信息
  console.log('提交数据:', formModel.value)
}
</script>

<template>
  <page-container title="基本资料">
    <!-- 表单部分 -->

    <el-form
      ref="formRef"
      :model="formModel"
      :rules="rules"
      label-width="100px"
      style="max-width: 500px"
    >
      <el-form-item label="登录名称">
        <el-input v-model="formModel.username" disabled></el-input>
      </el-form-item>
      <el-form-item label="用户昵称" prop="nickname">
        <el-input v-model="formModel.nickname"></el-input>
      </el-form-item>
      <el-form-item label="用户邮箱" prop="email">
        <el-input v-model="formModel.email"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm">提交修改</el-button>
      </el-form-item>
    </el-form>
  </page-container>
</template>
