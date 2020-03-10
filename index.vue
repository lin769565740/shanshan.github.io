<template>
  <div class="wrap">
    <div class="play_wrap" id="player">
      <div class="search_bar">
        <img src="./images/player_title.png" alt />
        <input type="text" v-model="inputValue" @keyup.enter="search" />
      </div>
      <div class="center_con">
        <!-- 歌曲列表 -->
        <list ref="list"></list>
        <!-- 播放动效 -->
        <cover ref="cover"></cover>
        <!-- 评论 -->
        <comment ref="comment"></comment>
      </div>
      <div class="audio_con">
        <audio
          :src="musicSrc"
          @play="playEvent"
          @pause="pauseEvent"
          controls
          autoplay
          loop
          class="myaudio"
        ></audio>
      </div>
    </div>
  </div>
</template>
<script>
import list from "./list.vue";
import cover from "./cover.vue";
import comment from "./comment.vue";
import axios from "axios";
export default {
  data() {
    return {
      inputValue: "", //输入框的值
      musicSrc: ""
    };
  },
  components: {
    list,
    cover,
    comment
  },
  methods: {
    //   音乐播放
    playEvent() {
      this.$refs.cover.playing = true;
    },
    // 音乐暂停
    pauseEvent() {
      this.$refs.cover.playing = false;
    },
    // 搜索歌曲
    search() {
      axios({
        url:
          "https://autumnfish.cn/search?keywords=" +
          this.inputValue +
          "&_t=" +
          Math.random() * 999
      }).then(res => {
        this.$refs.list.musicList = res.data.result.songs;
        this.$nextTick(() => {
          this.$refs.list.refreshScroll();
        });
      });
    },
    // 获取音乐播放地址
    playMusic(id) {
      axios({
        url: "https://autumnfish.cn/song/url?id=" + id
      }).then(res => {
        this.musicSrc = res.data.data[0].url;
        this.$refs.cover.playing = true;
      });
      this.getComment(id);
    },
    // 获取 评论
    getComment(id) {
      axios({ url: "https://autumnfish.cn/comment/music?id=" + id }).then(
        res => {
          this.$refs.comment.commentsList = res.data.comments;
          this.$nextTick(() => {
            this.$refs.comment.commentRefresh();
          });
        }
      );
    }
  }
};
</script>
<style>
body,
ul,
dl,
dd {
  margin: 0px;
  padding: 0px;
}

.wrap {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: url("images/bg.jpg") no-repeat;
  background-size: 100% 100%;
}

.play_wrap {
  width: 800px;
  height: 544px;
  position: fixed;
  left: 50%;
  top: 50%;
  margin-left: -400px;
  margin-top: -272px;
  /* background-color: #f9f9f9; */
}

.search_bar {
  height: 60px;
  background-color: #1eacda;
  overflow: hidden;
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
  z-index: 11;
}

.search_bar img {
  margin-left: 23px;
}

.search_bar input {
  margin-right: 23px;
  width: 296px;
  height: 34px;
  border-radius: 17px;
  border: 0px;
  background: url("images/zoom.png") 265px center no-repeat
    rgba(255, 255, 255, 0.45);
  text-indent: 15px;
  outline: none;
}

.center_con {
  height: 435px;
  background-color: rgba(255, 255, 255, 0.5);
  display: flex;
}

.song_wrapper {
  width: 200px;
  height: 435px;
  box-sizing: border-box;
  padding: 10px;
  list-style: none;
  background: url("images/line.png") right center no-repeat;
  position: relative;
  overflow: hidden;
}

.song_list li {
  font-size: 12px;
  color: #333;
  line-height: 36px;
  width: 180px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  cursor: pointer;
}

.song_list .active {
  color: #da651e;
}

.player_con {
  width: 400px;
  height: 435px;
  position: relative;
}

.disc {
  position: absolute;
  left: 73px;
  top: 60px;
  z-index: 9;
}

.cover {
  position: absolute;
  left: 125px;
  top: 112px;
  width: 150px;
  height: 150px;
  border-radius: 75px;
  z-index: 8;
}

.comment_list {
  width: 200px;
  height: 435px;
  box-sizing: border-box;
  padding: 10px;
  list-style: none;
  background: url("images/line.png") left center no-repeat;
  overflow: hidden;
  position: relative;
}

.comment_list dl {
  padding-left: 55px;
  position: relative;
  margin-bottom: 20px;
}

.comment_list dt {
  position: absolute;
  left: 4px;
  top: 0px;
}

.comment_list dt img {
  width: 40px;
  height: 40px;
  border-radius: 20px;
}

.comment_list dd {
  font-size: 12px;
}

.comment_list .name {
  font-weight: bold;
  color: #333;
  margin-top: 5px;
}

.comment_list .detail {
  color: #666;
  margin-top: 5px;
  line-height: 18px;
}

.audio_con {
  height: 50px;
  background-color: #f1f3f4;
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
}

.myaudio {
  width: 800px;
  height: 40px;
  margin-top: 5px;
  outline: none;
  background-color: #f1f3f4;
}

/* 旋转的动画 */
@keyframes Rotate {
  from {
    transform: rotateZ(0);
  }

  to {
    transform: rotateZ(360deg);
  }
}

/* 旋转的类名 */
.autoRotate {
  animation-name: Rotate;
  animation-iteration-count: infinite;
  animation-play-state: paused;
  animation-timing-function: linear;
  animation-duration: 5s;
}

/* 是否正在播放 */
.playing {
  animation-play-state: running;
}

.play_bar {
  position: absolute;
  left: 200px;
  top: -10px;
  z-index: 10;
  transform: rotate(-25deg);
  transform-origin: 12px 12px;
  transition: 1s;
}

/* 播放杆 转回去 */
.play_bar.playing {
  transform: rotate(0);
}
</style>