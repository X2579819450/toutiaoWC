<template>
  <div class="result">
    <van-list
      v-model="loading"
      @load="getResults"
      :finished="finished"
      finished-text="没有更多了"
      :error.sync="error"
      error-text="请求失败，点击重新加载"
    >
      <van-cell v-for="item in results" :key="item.art_id" :title="item.title" @click="$router.push({
        path:'/detail',
        query: { articleId: item.art_id}
      })">
      </van-cell>
    </van-list>
  </div>
</template>

<script>
import { getResultsAPI } from '@/api'
export default {
  props: {
    keywords: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      loading: false,
      page: 1,
      perPage: 10,
      results: [],
      finished: false,
      error: false
    }
  },
  methods: {
    async getResults() {
      // 发送请求
      try {
        // if (Math.random() < 0.1) {
        //   throw new Error()
        // }
        const { data } = await getResultsAPI(
          this.page++,
          this.perPage,
          this.keywords
        )
        console.log(1)
        if (data.data.results.length === 0) {
          this.finished = true
        }
        // 保存数据
        console.log(data)
        // this.results.push(...data.data.results)
        this.results = [...this.results, ...data.data.results]
      } catch {
        // console.log(error)
        this.error = true
      } finally {
        // 关闭loading
        this.loading = false
      }
    }
  }
}
</script>

<style scoped lang="less">
// overflow和auto用法一致的
.result {
  height:calc(100vh - 108px);
  overflow: auto;
}
</style>
