<script setup>
import { Delete, Edit } from '@element-plus/icons-vue'
import { ref } from 'vue'
import ChannelSelect from './components/ChannelSelect.vue'
import { artGetArticlesService } from '@/api/article'
import { formatTime } from '@/utils/format'

const articleList = ref([]) //文章列表数据
const total = ref(0) //总条数
const loading = ref(false) //加载状态
//请求参数对象
const params = ref({
  pagenum: 1,
  pagesize: 5,
  cate_id: '',
  state: ''
})
//发送请求获取文章列表
const getArticleList = async () => {
  loading.value = true
  const res = await artGetArticlesService(params.value)
  articleList.value = res.data.data
  total.value = res.data.total
  loading.value = false
}
getArticleList()
//处理分页逻辑
const onSizeChange = (size) => {
  params.value.pagenum = 1 //每次修改页码大小时，重置页码为1
  params.value.pagesize = size
  getArticleList()
}
const onCurrentChange = (page) => {
  params.value.pagenum = page
  getArticleList()
}
//搜索逻辑=>按最新的条件重新检索
const onSearch = () => {
  params.value.pagenum = 1 //每次搜索时，重置页码为1
  getArticleList()
}
//重置逻辑
const onReset = () => {
  params.value.cate_id = ''
  params.value.state = ''
  params.value.pagenum = 1 //重置页码为1
  getArticleList()
}
//控制抽屉显示隐藏
const visibleDrawer = ref(false)
//添加逻辑
const onAddArticle = () => {
  visibleDrawer.value = true
}
//编辑逻辑
const onEditArticle = (row) => {
  console.log('编辑文章', row)
}
//删除逻辑
const onDeleteArticle = (row) => {
  console.log('删除文章', row)
}
</script>

<template>
  <PageContainer title="文章管理">
    <template #extra>
      <el-button type="primary" @click="onAddArticle">添加文章</el-button>
    </template>

    <!-- 表单区域 -->
    <el-form inline>
      <el-form-item label="文章分类:" style="width: 200px">
        <ChannelSelect v-model="params.cate_id"></ChannelSelect>
      </el-form-item>
      <el-form-item label="发布状态:" style="width: 200px">
        <el-select v-model="params.state">
          <el-option label="已发布" value="已发布"></el-option>
          <el-option label="草稿" value="草稿"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button @click="onSearch" type="primary">筛选</el-button>
      </el-form-item>
      <el-form-item>
        <el-button @click="onReset">重置</el-button>
      </el-form-item>
    </el-form>
    <!-- 表格区域 -->
    <el-table :data="articleList" v-loading="loading">
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
            @click="onEditArticle(row)"
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
    <!-- 分页区域 -->
    <el-pagination
      v-model:current-page="params.pagenum"
      v-model:page-size="params.pagesize"
      :page-sizes="[1, 2, 4, 5, 10]"
      layout="jumper, total, sizes, prev, pager, next"
      :background="true"
      :total="total"
      @size-change="onSizeChange"
      @current-change="onCurrentChange"
      style="margin-top: 20px; justify-content: flex-end"
    ></el-pagination>
    <!-- 抽屉 -->
    <el-drawer
      v-model="visibleDrawer"
      title="大标题"
      direction="rtl"
      size="50%"
    >
      <span>抽屉内容</span>
    </el-drawer>
  </PageContainer>
</template>

<style lang="scss" scoped></style>
