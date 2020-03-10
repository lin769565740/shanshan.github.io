<template>
  <div class="player_con">
    <img src="./images/player_bar.png" class="play_bar" :class="{playing:playing}" />
    <!-- 黑胶碟片 -->
    <img src="./images/disc.png" class="disc autoRotate" :class="{playing:playing}" />
    <img src="./images/cover.png" class="cover autoRotate" :class="{playing:playing}" />
  </div>
</template>
<script>
export default {
  data() {
    return {
      playing: false
    };
  }
};
</script>
<style>
</style>