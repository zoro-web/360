<template>
  <div class="comment">
    <div class="info">
      <span class="triangle-up" v-show="show"></span>
      {{ comment.author }} {{ comment.time }}
      <span @click="show = !show">[-]</span>
    </div>
    <transition name="opacity">
      <div v-if="show">
        <div class="content">
          {{ comment.content }}
        </div>
        <button @click="replyFn">reply</button>
        <el-dialog
          title="回复"
          :visible.sync="replyVisible">
          <textarea v-model="replyValue" cols="30" rows="10"></textarea>
          <button @click="saveFn">回复</button>
        </el-dialog>
        <template v-if="comment.children && comment.children.length > 0">
          <comment v-for="(item, index) in comment.children" :key="index" :comment="item"></comment>
        </template>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'comment',
  props: {
    comment: {
      type: Object,
      default: () => {
        return {}
      }
    }
  },
  data() {
    return {
      replyVisible: false,
      replyValue: '',
      show: true
    }
  },
  components: {},
  methods: {
    replyFn() {
      this.replyVisible = true
    },
    saveFn() {
      if (!this.comment.children || this.comment.children.length === 0) {
        this.$set(this.comment, 'children', [])
      }
      this.comment.children.unshift({
        author: 'zoro',
        time: '4 hours age', // 前端取的本地时间可以随意更改，不准确，多少个小时前由服务器计算精确
        content: this.replyValue
      })
      this.replyValue = ''
      this.replyVisible = false
    }
  },
  created() {},
  mounted() {}
}
</script>

<style lang="scss" scoped>
  @import "@/styles/mixin.scss";
  .comment {
    margin-left: 20px;
    .content {
      margin-bottom: 5px;
    }
  }
  .triangle-up {
    @include triangle(#ccc, 7px, top);
    position: relative;
    bottom: 8px;
    right: 5px;
  }
  .opacity-enter-active, .opacity-leave-active {
    transition: opacity .3s;
  }
  .opacity-enter, .opacity-leave-to {
    opacity: 0;
  }
</style>