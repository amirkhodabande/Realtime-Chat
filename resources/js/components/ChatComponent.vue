<template>
      <div class=" m-auto col-4">
            <li class="list-group-item active rounded-top">Chatroom</li>
            <ul
                  class="list-group"
                  v-chat-scroll
            >
                  <message
                        v-for="data in chat.messages"
                        :key="data.index"
                        color="warning"
                  >
                        {{ data.message }}
                  </message>
            </ul>
            <input
                  type="text"
                  @keyup.enter="send"
                  v-model="message"
                  class="list-group-item form-control rounded-0"
                  placeholder="Type your message here..."
            >
      </div>
</template>

<script>
import Vue from "vue";
import VueChatScroll from "vue-chat-scroll";
import Message from "./children/Message.vue";
Vue.use(VueChatScroll);
export default {
      components: {
            message: require("./children/Message.vue").default,
      },
      mounted() {
            this.listen();
      },
      data() {
            return {
                  message: "",
                  chat: {
                        messages: [],
                  },
            };
      },
      methods: {
            listen() {
                  Echo.private("chat").listen("NewMessage", (response) => {
                        this.chat.messages.push(response);
                  });
            },
            send() {
                  if (this.message.length > 0) {
                        this.chat.messages.push(this.message);
                        this.message = "";
                  }
            },
      },
};
</script>

<style>
.list-group {
      overflow-y: scroll;
      height: 250px;
      background-color: rgba(243, 220, 184, 0.24);
}
</style>
