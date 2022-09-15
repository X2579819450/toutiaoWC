<template>
  <div>
    <form action="/">
      <van-search
        class="search"
        v-model.trim="keywords"
        background="#3296fa"
        placeholder="请输入搜索关键词"
        show-action
        @search="onSearch"
        @focus="isShowSearchResult = false"
        @cancel="$router.push('/')"
      />
    </form>
    <!-- 搜索历史/建议/结果 -->
    <!-- <search-history></search-history>
    <search-result></search-result>
    <search-suggestion></search-suggestion> -->
    <component
      :keywords="keywords"
      :is="componentName"
      @change-keywords="onSearch"
    ></component>
  </div>
</template>

<script>
import SearchHistory from './components/SearchHistory.vue'
import SearchResult from './components/SearchResult.vue'
import SearchSuggestion from './components/SearchSuggestion.vue'
import { mapMutations, mapState } from 'vuex'
export default {
  data() {
    return {
      keywords: '',
      isShowSearchResult: false
    }
  },
  components: {
    SearchHistory,
    SearchResult,
    SearchSuggestion
  },
  computed: {
    ...mapState(['histories']),
    componentName() {
      // 搜索历史：搜索框的值空
      if (this.keywords === '') {
        return 'SearchHistory'
      }
      // this.keywords一定有值
      if (this.isShowSearchResult) {
        return 'SearchResult'
      }

      return 'SearchSuggestion'
    }
  },
  methods: {
    ...mapMutations(['SET_HISTORIES']),
    onSearch(keywords) {
      // 把keywords关键词存起来
      // 1. 用户手动敲回车 2. 点击了搜索建议
      // 存储搜索历史

      // 去重：1. 获取没有去重的数组 2. 放在new Set(arr) 3.[...set]
      const distinctHistories = [...new Set([keywords, ...this.histories])]
      this.SET_HISTORIES(distinctHistories)
      this.keywords = keywords
      this.isShowSearchResult = true
    }
  }
}
</script>

<style lang="less" scoped>
.search {
  [role='button'] {
    color: #fff;
  }
}
</style>
