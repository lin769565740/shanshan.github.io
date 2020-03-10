<template>
  <div class="comment_list" ref="comment_list">
    <div>
      <dl v-for="(item, index) in commentsList" :key="index">
        <dt>
          <img :src="item.user.avatarUrl" alt />
        </dt>
        <dd class="name">{{item.user.nickname}}</dd>
        <dd class="detail">{{item.content}}</dd>
      </dl>
    </div>
  </div>
</template>
<script>
import IScroll from "iscroll";
export default {
  data() {
    return {
      commentsList: [],
      commentScroll: ""
    };
  },
  methods: {
    commentRefresh() {
      this.commentScroll.refresh();
    }
  },
  mounted() {
    this.commentScroll = new IScroll(this.$refs.comment_list, {
      scrollbars: true
    });
  }
};
</script>
<style>
</style>