<template>
  <div>
    <div class="add-comment">
      <textarea v-model="commentValue" cols="30" rows="10"></textarea>
      <div>
        <button @click="addCommentFn">add comment</button>
      </div>
    </div>
    <div class="comment-list">
      <comment v-for="(item, index) in commentList" :key="index" :comment="item">
      </comment>
    </div>
  </div>
</template>

<script>
// 我这边https://github.com/HackerNews/API的api地址打不开，在本地实现功能
import comment from './components/comment'
export default {
  data() {
    return {
      commentValue: '',
      commentList: [{
        content: '<script>alert(1)<script>',
        author: 'zoro',
        time: '8 hours age', // 前端取的本地时间可以随意更改，不准确，多少个小时前由服务器计算精确
        children: [{
          author: 'zoro',
          time: '4 hours age', // 前端取的本地时间可以随意更改，不准确，多少个小时前由服务器计算精确
          content: '1-1'
        }]
      }]
    }
  },
  components: {
    comment
  },
  methods: {
    addCommentFn() {
      if (this.commentValue.length === 0) {
        alert('请填写评论')
      }
      this.commentList.unshift({
        content: this.commentValue,
        author: 'zoro',
        time: '16 hours age', // 前端取的本地时间可以随意更改，不准确，多少个小时前由服务器计算精确
      })
      this.commentValue = ''
    }
  },
  created() {},
  mounted() {}
}
</script>

<style lang='scss' scoped>
  .add-comment {
    margin-bottom: 30px;
  }
</style>