<template>
  <div id="app">
    <div class="header">
      <div class="right">
        <i class="el-icon-minus min"></i>
        <i class="el-icon-full-screen max"></i>
        <!--el-icon-copy-document-->
        <i class="el-icon-close close"></i>
      </div>
      <div class="title">Title</div>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "shyfcka_chat_room",
  created() {
    window.vm = this;
  },
};
</script>

<style>
html,
body {
  padding: 0;
  margin: 0;
  width: 100vw;
  height: 100vh;
  user-select: none;
  font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB",
    "Microsoft YaHei", "微软雅黑", Arial, sans-serif;
  background: radial-gradient(ellipse at top left, #fff 40%, #c3c3c3 100%);
}
body {
  box-sizing: border-box;
  border: 1px #409eff solid;
}
.header {
  background-color: #409eff;
  height: 30px;
  -webkit-app-region: drag;
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
  display: block;
  float: right;
  height: 30px;
  text-align: center;
}
.header .right i {
  cursor: pointer;
  width: 46px;
  height: 100%;
  display: inline-block;
  line-height: 30px;
  text-align: center;
  transition-duration: 0.3s;
}
.header .right min:hover,
.header .right max:hover {
  background-color: #9fcfff;
}
</style>
