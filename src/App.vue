<template>
  <div>
    <button @click="getGoodsList200">更新数据</button>
    <div style="height: 1px; margin-top: 10px"></div>
    <scroll
      class="wrapper"
      ref="wrapper"
      :listenScroll="true"
      :pullup="true"
      :data="lists"
      @scrollToEnd="scrollToEnd"
      @setScroll="setScroll"
      @beforeScroll="beforeScroll"
      @scroll="scroll"
    >
      <ul @click="ulClick">
        <li v-for="(item, index) in lists" v-bind:key="index">"{{ item }}"</li>
      </ul>
    </scroll>
  </div>
</template>

<script>
import scroll from "./components/common/scroll/Scroll";
import axios from "axios";

export default {
  name: "App",
  components: {
    scroll,
  },
  data() {
    return {
      lists: null,
    };
  },
  methods: {
    scrollToEnd(scroll) {
      this.scroll = scroll;
      console.log("下拉到最底下");
    },
    setScroll(scroll) {
      this.scroll = scroll;
      console.log("scroll创建成功");
    },
    scroll(pos) {
      console.log(pos); //监听滚动坐标
    },
    beforeScroll() {
      console.log("滚动之前");
    },

    async getGoodsList() {
      const { data: res } = await axios.get("http://localhost:9999/");
      this.lists = res.data;
      /* 
        Scroll内部用watch来侦测lists变化，一旦变化，延时20ms刷新dom，否则通过下面的钩子需要手动刷新

        // this.$nextTick(function () {
        //   this.$refs.wrapper.refresh();
        // });
      */
    },
    async getGoodsList200() {
      const { data: res } = await axios.get("http://localhost:9999/aaa");
      this.lists = res.data;
      /* 
        Scroll内部用watch来侦测lists变化，一旦变化，延时20ms刷新dom，否则通过下面的钩子需要手动刷新

        // this.$nextTick(function () {
        //   this.$refs.wrapper.refresh();
        // });
      */
    },

    ulClick(){
      console.log("点击了");
    }
  },
  mounted() {
    this.getGoodsList();
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.wrapper {
  background: pink;
  height: 350px;
  overflow: hidden;
}
</style>
