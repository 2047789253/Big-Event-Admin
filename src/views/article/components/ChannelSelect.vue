<script setup>
import { artGetChannelsService } from '@/api/article'
import { ref } from 'vue'

const channelList = ref([])
const getChannelList = async () => {
  const res = await artGetChannelsService()
  channelList.value = res.data.data
  console.log(channelList.value)
}
getChannelList()
</script>

<template>
  <el-select>
    <el-option label="新闻" value="110"></el-option>
    <el-option label="科技" value="121"></el-option>
  </el-select>
</template>
