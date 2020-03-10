<template>
  <div class="song_wrapper" ref="song_wrapper">
    <ul class="song_list">
      <li v-for="(item, index) in musicList" :key="index" @click="musicClick(item)">{{item.name}}</li>
    </ul>
  </div>
</template>
<script>
import IScroll from "iscroll";
export default {
  data() {
    return {
      iscroll: "",
      musicList: []
    };
  },
  methods: {
    refreshScroll() {
      this.iscroll.refresh();
    },
    musicClick(item) {
      this.$parent.playMusic(item.id);
      //   window.console.log(this.$parent, item);
    }
  },
  mounted() {
    this.iscroll = new IScroll(this.$refs.song_wrapper, {
      scrollbars: true
    });
  }
};
</script>
<style>
</style>