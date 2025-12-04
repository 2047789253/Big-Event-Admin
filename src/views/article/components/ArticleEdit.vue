<script setup>
import { ref } from 'vue'
import ChannelSelect from './ChannelSelect.vue'
import { Plus } from '@element-plus/icons-vue'
import { QuillEditor } from '@vueup/vue-quill'
import '@vueup/vue-quill/dist/vue-quill.snow.css'
import {
  artPublishService,
  artGetDetailService,
  artEditService
} from '@/api/article.js'
import { ElMessage } from 'element-plus'
import { baseURL } from '@/utils/request'
import axios from 'axios'
// 控制抽屉显示隐藏
const visible = ref(false)
//默认数据为空
const defaultFormModel = {
  title: '',
  cate_id: '',
  cover_img: '',
  content: '',
  state: ''
}
//准备数据
const formModel = ref({ ...defaultFormModel })
//图片上传相关逻辑
const imgUrl = ref('')
const onUploadFile = (file) => {
  imgUrl.value = URL.createObjectURL(file.raw) // 预览图片用
  formModel.value.cover_img = file.raw // 存储原始文件对象，用于提交
}
//发表文章逻辑
const emit = defineEmits(['success'])
const onPublish = async (state) => {
  formModel.value.state = state //设置文章状态
  //准备formData对象
  const fd = new FormData()
  for (let key in formModel.value) {
    fd.append(key, formModel.value[key])
  }
  //发请求
  if (formModel.value.id) {
    // 编辑文章逻辑
    await artEditService(fd)
    ElMessage.success('修改文章成功')
    visible.value = false
    emit('success', 'edit') //通知父组件刷新列表
  } else {
    // 添加文章逻辑
    await artPublishService(fd)
    ElMessage.success('添加文章成功')
    visible.value = false
    emit('success', 'add') //通知父组件刷新列表
  }
}
//回显
const editorRef = ref()
const open = async (row) => {
  visible.value = true
  if (row.id) {
    const res = await artGetDetailService(row.id)
    formModel.value = { ...res.data.data }
    //图片单独出来处理
    const imageUrl = baseURL + formModel.value.cover_img
    imgUrl.value = imageUrl
    //网址转为file对象
    try {
      const response = await axios.get(imageUrl, { responseType: 'blob' })
      const blob = response.data
      const fileName = formModel.value.cover_img.split('/').pop() || 'cover.jpg'
      const file = new File([blob], fileName, { type: blob.type })
      formModel.value.cover_img = file
    } catch (error) {
      console.error('Failed to convert URL to file:', error)
      ElMessage.error('加载封面图片失败')
    }
  } else {
    formModel.value = { ...defaultFormModel }
    imgUrl.value = ''
    editorRef.value.setHTML('') // 清空富文本编辑器内容
    console.log('添加')
  }
}
defineExpose({ open })
</script>

<template>
  <el-drawer
    v-model="visible"
    :title="formModel.id ? '编辑文章' : '添加文章'"
    direction="rtl"
    size="50%"
  >
    <!-- 发表文章表单 -->
    <el-form :model="formModel" ref="formRef" label-width="100px">
      <el-form-item label="文章标题" prop="title">
        <el-input v-model="formModel.title" placeholder="请输入标题"></el-input>
      </el-form-item>
      <el-form-item label="文章分类" prop="cate_id">
        <channel-select
          v-model="formModel.cate_id"
          style="width: 100%"
        ></channel-select>
      </el-form-item>
      <el-form-item label="文章封面" prop="cover_img">
        <el-upload
          class="avatar-uploader"
          :auto-upload="false"
          :show-file-list="false"
          :on-change="onUploadFile"
        >
          <img v-if="imgUrl" :src="imgUrl" class="avatar" />
          <el-icon v-else class="avatar-uploader-icon"><Plus /></el-icon>
        </el-upload>
      </el-form-item>
      <el-form-item label="文章内容" prop="content">
        <div class="editor">
          <QuillEditor
            ref="editorRef"
            theme="snow"
            v-model:content="formModel.content"
            content-type="html"
          >
          </QuillEditor>
        </div>
      </el-form-item>
      <el-form-item>
        <el-button @click="onPublish('已发布')" type="primary">发布</el-button>
        <el-button @click="onPublish('草稿')" type="info">草稿</el-button>
      </el-form-item>
    </el-form>
  </el-drawer>
</template>

<style lang="scss" scoped>
.avatar-uploader {
  :deep() {
    .avatar {
      width: 178px;
      height: 178px;
      display: block;
    }
    .el-upload {
      border: 1px dashed var(--el-border-color);
      border-radius: 6px;
      cursor: pointer;
      position: relative;
      overflow: hidden;
      transition: var(--el-transition-duration-fast);
    }
    .el-upload:hover {
      border-color: var(--el-color-primary);
    }
    .el-icon.avatar-uploader-icon {
      font-size: 28px;
      color: #8c939d;
      width: 178px;
      height: 178px;
      text-align: center;
    }
  }
}
.editor {
  width: 100%;
  :deep(.ql-editor) {
    min-height: 200px;
  }
}
</style>
