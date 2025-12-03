<script setup>
import { artGetChannelsService } from '@/api/article'
import { ref } from 'vue'
//传入数据子传父
const modelValue = defineModel({
  type: [Number, String]
})
// 获取文章分类列表
const channelList = ref([])
const getChannelList = async () => {
  const res = await artGetChannelsService()
  channelList.value = res.data.data
}
getChannelList()
</script>

<template>
  <el-select v-model="modelValue" placeholder="请选择文章分类" clearable>
    <el-option
      v-for="channel in channelList"
      :key="channel.id"
      :label="channel.cate_name"
      :value="channel.id"
    ></el-option>
  </el-select>
</template>
