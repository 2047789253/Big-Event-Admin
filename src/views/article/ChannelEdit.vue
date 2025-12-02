<script setup>
import { ref } from 'vue'
const dialogVisible = ref(false)
const formModel = ref({
  cate_name: '',
  cate_alias: ''
})
const rules = {
  cate_name: [
    { required: true, message: '分类名称不能为空', trigger: 'blur' },
    {
      pattern: /^\S{2,12}$/,
      message: '分类名称长度在2-12个字符之间且非空',
      trigger: 'blur'
    }
  ],
  cate_alias: [
    { required: true, message: '分类别名不能为空', trigger: 'blur' },
    {
      pattern: /^[a-zA-Z0-9]{1-15}$/,
      message: '分类必须为1-15位字母或数字',
      trigger: 'blur'
    }
  ]
}
const open = (row) => {
  dialogVisible.value = true
  console.log(row)
  formModel.value = { ...row }
}
defineExpose({ open })
</script>

<template>
  <el-dialog v-model="dialogVisible" title="添加弹层" width="30%">
    <el-form
      :model="formModel"
      :rules="rules"
      label-width="100px"
      style="padding-right: 30px"
    >
      <el-form-item label="分类名称" prop="cate_name">
        <el-input v-model="formModel.cate_name" placeholder="请输入分类名称" />
      </el-form-item>
      <el-form-item label="分类别名" prop="cate_alias">
        <el-input v-model="formModel.cate_alias" placeholder="请输入分类别名" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="dialogVisible = false">
          确认
        </el-button>
      </span>
    </template>
  </el-dialog>
</template>
