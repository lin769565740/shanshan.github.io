<template>
  <!-- 
在组件中使用axios
1:装包，在相应目录安装  npm i axios
2:导包   import axios(这个名字可以随便写) from 'axios(这个名字就是我们安装时的名字)'
3：用包  注意点 使用的是import 的名字是什么，就用什么名字
  -->
  <div class="main">
    <input type="text" v-model="inputValue" />
    <button @click="btnClick">点我啊</button>
    <ul class="list">
      <li v-for="(item, index) in list" :key="index" @click="getsrc(item.id)">{{item.name}}</li>
    </ul>
    <audio :src="musicSrc" controls autoplay></audio>
  </div>
</template>
<script>
// i
import axios from "axios";
export default {
  data() {
    return {
      inputValue: "",
      list: [],
      musicSrc: ""
    };
  },
  methods: {
    btnClick() {
      axios({
        url:
          "https://autumnfish.cn/search?keywords=" +
          this.inputValue +
          "&t=" +
          Math.random() * 999
      }).then(res => {
        this.list = res.data.result.songs;
        //   打印的时候注意了，语法严格一些了，console.log前面一定要加上window
        window.console.log(res);
      });
    },
    getsrc(id) {
      window.console.log(id);
      axios({
        url: "https://autumnfish.cn/song/url?id=" + id
      }).then(res => {
        this.musicSrc = res.data.data[0].url;
        window.console.log("歌曲url", res);
      });
    }
  }
};
</script>
<style>
* {
  padding: 0;
  margin: 0;
  list-style: none;
}
.main {
  width: 600px;
  margin: 0 auto;
  border: 1px solid #ccc;
}
.list {
  width: 100%;
  height: 500px;
  overflow: auto;
}
</style>