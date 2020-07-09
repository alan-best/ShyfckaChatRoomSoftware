<template>
  <div id="app">
    <div class="header">
      <div class="right">
        <i class="el-icon-minus min" @click="min"></i>
        <i
          :class="{
            'el-icon-full-screen': isMaxIcon,
            'el-icon-copy-document': !isMaxIcon,
            max: true,
          }"
          @click="max"
        ></i>
        <!--el-icon-copy-document-->
        <i class="el-icon-close close" @click="close"></i>
      </div>
      <div class="title">shyfcka 聊天室</div>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
import { remote } from "electron";

export default {
  name: "shyfcka_chat_room",
  created() {
    window.vm = this;
  },
  data() {
    return {
      isMaxIcon:true
    }
  },
  methods: {
    min() {
      remote.getCurrentWindow().minimize();
    },
    max() {
      if (remote.getCurrentWindow().isMaximized()) {
        remote.getCurrentWindow().restore();
        this.isMaxIcon=true
      } else {
        remote.getCurrentWindow().maximize();
        this.isMaxIcon=false
      }
    },
    close() {
      remote.getCurrentWindow().close();
    },
  },
};
</script>

<style>
html,
body,
#app {
  padding: 0;
  margin: 0;
  width: 100vw;
  height: 100vh;
  user-select: none;
  font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB",
    "Microsoft YaHei", "微软雅黑", Arial, sans-serif;
  background: radial-gradient(ellipse at top left, #fff 40%, #c3c3c3 100%);
  cursor: default;
  outline: none;
  overflow: hidden;
}
#app {
  border: 1px #409eff solid;
  width: calc(100vw - 2px);
  height: calc(100vh - 2px);

}
.header {
  background-color: #409eff;
  height: 30px;
  -webkit-app-region: drag;
  cursor: default;
  color: #fff;
}
.header .title {
  text-align: center;
  height: 30px;
  width: calc(100% - (46px * 3));
  line-height: 30px;
}
.header .right {
  top: 0px;
  -webkit-app-region: no-drag;
  float: right;
  height: 30px;
  text-align: center;
}
.header .right i {
  cursor: pointer;
  width: 46px;
  height: 30px;
  line-height: 30px;
  transition-duration: 0.3s;
}
.header .right .min:hover,
.header .right .max:hover {
  background-color: #8ac5ff;
}
.header .right .close:hover {
  background-color: red;
}
</style>
