<script setup>
import { Delete, Edit } from '@element-plus/icons-vue'
import { ref } from 'vue'
import ChannelSelect from './components/ChannelSelect.vue'
import { artGetArticlesService } from '@/api/article'
import { formatTime } from '@/utils/format'

const articleList = ref([]) //文章列表数据
const total = ref(0) //总条数
//请求参数对象
const params = ref({
  pagenum: 1,
  pagesize: 5,
  cate_id: '',
  state: ''
})
//发送请求获取文章列表
const getArticleList = async () => {
  const res = await artGetArticlesService(params.value)
  articleList.value = res.data.data
  total.value = res.data.total
}
getArticleList()

const onEditAriticle = (row) => {
  console.log('编辑文章', row)
}
const onDeleteArticle = (row) => {
  console.log('删除文章', row)
}
</script>

<template>
  <PageContainer title="文章管理">
    <template #extra>
      <el-button>添加文章</el-button>
    </template>

    <!-- 表单区域 -->
    <el-form inline>
      <el-form-item label="文章分类:" style="width: 200px">
        <ChannelSelect v-model="params.cate_id"></ChannelSelect>
      </el-form-item>
      <el-form-item label="发布状态:" style="width: 200px">
        <el-select v-model="params.state">
          <el-option label="已发布" value="已发布"></el-option>
          <el-option label="待审核" value="待审核"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary">筛选</el-button>
      </el-form-item>
      <el-form-item>
        <el-button>重置</el-button>
      </el-form-item>
    </el-form>
    <!-- 表格区域 -->
    <el-table :data="articleList">
      <el-table-column label="文章标题" prop="title">
        <template #default="{ row }">
          <el-link type="primary" underline="false">{{ row.title }}</el-link>
        </template>
      </el-table-column>
      <el-table-column label="分类" prop="cate_name"></el-table-column>
      <el-table-column label="发布时间" prop="pub_date">
        <template #default="{ row }">
          {{ formatTime(row.pub_date) }}
        </template>
      </el-table-column>
      <el-table-column label="状态" prop="state"></el-table-column>
      <el-table-column label="操作">
        <template #default="{ row }">
          <el-button
            circle
            plain
            type="primary"
            :icon="Edit"
            @click="onEditAriticle(row)"
          ></el-button>
          <el-button
            circle
            plain
            type="danger"
            :icon="Delete"
            @click="onDeleteArticle(row)"
          ></el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      style="margin-top: 20px; text-align: right"
      background
      layout="prev, pager, next, jumper"
      :total="total"
    ></el-pagination>
  </PageContainer>
</template>

<style lang="scss" scoped></style>
