<template>
      <div class=" m-auto col-sm-10 col-md-5 ">
            <li class="list-group-item active rounded-top">Chatroom
                  <small
                        v-if="typing == true"
                        class="badge badge-info text-white"
                  ><i>Typing...</i></small>
            </li>
            <ul
                  class="list-group"
                  v-chat-scroll
            >
                  <message
                        v-for="data in chat.messages"
                        :key="data.index"
                  >
                        <template #message>{{ data.message }}</template>
                        <template #user>{{ data.user }}</template>
                        <template #time>{{ data.time }}</template>
                        <template
                              #color
                              v-if="data.user == 'You'"
                        >success</template>
                        <template
                              #color
                              v-else
                        >warning</template>
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
                  typing: false,
            };
      },
      watch: {
            message() {
                  Echo.private("chat").whisper("typing", {
                        name: this.message,
                  });
            },
      },
      methods: {
            listen() {
                  Echo.private("chat")
                        .listen("NewMessage", (response) => {
                              this.chat.messages.push(response);
                        })
                        .listenForWhisper("typing", (e) => {
                              if (e.name != "") {
                                    this.typing = true;
                              } else {
                                    this.typing = false;
                              }
                        });
            },
            getComments() {
                  axios.get(`/chat`)
                        .then((response) => {
                              this.chat.messages = response.data;
                        })
                        .catch(function (error) {
                              console.log(
                                    "Error, we cant load messages! Please refresh your page."
                              );
                        });
            },
            send() {
                  if (this.message.length > 0) {
                        axios.post(`/chat`, {
                              message: this.message,
                              time: this.getTime(),
                        })
                              .then((response) => {
                                    this.chat.messages.push({
                                          message: this.message,
                                          user: "You",
                                          time: this.getTime(),
                                    });
                                    this.message = "";
                              })
                              .catch((error) => {
                                    console.log("Error, please try later.");
                              });
                  }
            },
            getTime() {
                  let time = new Date();
                  return time.getHours() + ":" + time.getMinutes();
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
