<template>
  <div class="chat-container" id="chat-container">
    <div class="chat-message" v-for="msg in msgs" v-bind:key="msg">
      <span class="message-author" :style="{ color: msg[0]['color'] }">
        {{ msg[0]["display-name"] }}
      </span>
      :
      <span class="message-text">{{ msg[1] }}</span>
    </div>
  </div>
</template>

<script>
import tmi from "tmi.js";

export default {
  name: "Chat",
  data() {
    return {
      client: null,
      msgs: [],
    };
  },
  watch: {
    msgs: {
      // eslint-disable-next-line no-unused-vars
      handler(val, oldVal) {
        const chatContainer = document.querySelector("#chat-container");
        chatContainer.scrollTop = chatContainer.scrollHeight + 100;
      },
      deep: true,
    },
  },
  mounted() {
    this.client = new tmi.Client({
      channels: ["michaelreeves"],
    });

    this.client.connect();

    // eslint-disable-next-line no-unused-vars
    this.client.on("message", (channel, tags, message, _self) => {
      this.msgs.push([tags, message]);

      if (this.msgs.length > 50) {
        this.msgs.shift();
      }
    });
  },
};
</script>

<style lang="scss">
.chat-container {
  border: 1px solid black;
  width: 30vw;
  max-width: 400px;
  height: 70vh;

  padding: 20px;
  padding-bottom: 50px;

  overflow-y: hidden;
  overflow-x: hidden;

  text-align: left;

  .chat-message {
    .message-author {
      font-weight: bold;
    }

    // &:nth-of-type(2n) {
    //   background: #ffffff;
    // }

    // &:nth-of-type(2n + 1) {
    //   background: #fff000;
    // }
  }

  &:hover {
    overflow-y: scroll;
    transition: overflow-y 2s;
  }
}
</style>
