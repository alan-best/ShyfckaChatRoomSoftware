<template>
  <div id="page-Home" v-loading="loading" element-loading-text="正在连接服务器">
    <el-timeline>
      <el-timeline-item
        v-for="(activity, index) in chat"
        :key="index"
        :icon="activity.icon"
        :type="activity.type"
        :color="activity.color"
        :size="activity.size"
        :timestamp="activity.time"
      >
        <el-tag v-if="activity.tag" :type="activity.tag.type" size="mini">{{
          activity.tag.name
        }}</el-tag>
        {{ activity.content }}
      </el-timeline-item>
    </el-timeline>
    <div class="sendArea">
      <el-input
        id="input"
        v-model="inputValue"
        placeholder="说点什么吧"
        type="textarea"
        resize="none"
        maxlength="300"
        show-word-limit
      ></el-input
      ><el-button type="primary" :disabled="sendDisbled" @click="send()"
        >发送</el-button
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      userName: null,
      loading: true,
      chat: [{ color: "#67C23A", content: "你进入了聊天室" }],
      inputValue: "",
    };
  },
  computed: {
    sendDisbled() {
      if (this.inputValue == "") {
        return true;
      } else {
        return false;
      }
    },
  },
  created() {
    this.$prompt(
      "输入一个用户名,这个名字将是此次聊天的用户名",
      "输入用户名以继续",
      {
        showCancelButton: false,
        inputPattern: /^(\w){4,18}$/,
        inputErrorMessage: "只能包含字母,数字或下划线,长度为4~18",
        beforeClose: (act, ins, done) => {
          if (act != "confirm") {
            return false;
          } else {
            if (ins.inputValue.length) {
              this.userName = ins.inputValue;
              done();
            }
          }
        },
        showClose: false,
        callback: this.steup,
      }
    );
  },
  methods: {
    send() {
      this.ws.send(
        JSON.stringify({
          type: "message",
          writer: this.userName,
          data: this.inputValue,
        })
      );
      this.inputValue = "";
    },
    socket() {
      let ws = new WebSocket("ws://115.52.161.139:2333");
      this.ws = ws;
      let _this = this;
      ws.onopen = () => {
        _this.loading = false;
        this.$notify({
          dangerouslyUseHTMLString: true,
          title: "已连接到服务器",
          position: "bottom-right",
          message: "已连接websocket<br/>用户名:" + this.userName,
          type: "success",
          duration: 2e3,
        });
        ws.send(
          JSON.stringify({
            type: "command",
            data: "login",
            data2: _this.userName,
          })
        );
      };
      ws.onmessage = (m) => {
        if (_this.chat.length > 300) {
          _this.chat.splice(0);
        }
        if (m.data == "ping") {
          ws.send("pong");
          return;
        }
        let dat = JSON.parse(m.data);

        let msg = dat.data;
        let opt = { content: msg };
        if (dat.writer == "SYSTEM") {
          opt.tag = { name: "系统" };
          opt.color = "#4093ff";
        } else {
          opt.tag = { name: dat.writer, type: "success" };
        }
        _this.chat.push(opt);
        document.querySelector(".el-timeline").scroll({ top: 99999 });
      };
      ws.onclose = () => {
        ws = null;
        this.$alert("服务器断开连接,点击确定重连", {
          type: "error",
          title: "连接丢失",
        }).then(() => location.reload());
      };
    },
    steup() {
      setTimeout(() => {
        this.socket();
      }, 1000);
    },
  },
};
</script>

<style>
#page-Home {
  padding-top: 50px;
  height: calc(100% - 30px - 50px);
}
.sendArea {
  bottom: 20px;
  position: absolute;
  padding: 20px 20px;
  width: calc(100% - 20px * 2);
}
.sendArea .el-button {
  width: 100%;
}
.el-timeline {
  overflow-y: auto;
  height: calc(100% - 150px);
  word-break: break-all;
  white-space: pre-line;
}
::-webkit-scrollbar {
  width: 5px;
}
</style>
