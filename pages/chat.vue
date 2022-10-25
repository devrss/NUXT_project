<template>
  <v-card class="overflow-hidden" height="500">
    <v-app-bar color="secondary" dark>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Комната {{user.room}}</v-toolbar-title>
      <v-spacer />
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" absolute temporary color="secondary" dark>
      <v-list nav dense rounded>
        <v-subheader>Люди онлайн</v-subheader>
        <v-list-item v-for="u in users" :key="u.id" @click.prevent>
          <v-list-item-content>
            <v-list-item-title>{{u.name}}</v-list-item-title>
          </v-list-item-content>

          <v-list-item-icon>
            <v-icon :color="u.id === user.id ? 'primary' : 'grey'">mdi-message</v-icon>
          </v-list-item-icon>
        </v-list-item>
      </v-list>

      <template v-slot:append>
        <div class="pa-2">
          <v-btn rounded color="primary" @click="exit" block>Выход</v-btn>
        </div>
      </template>
    </v-navigation-drawer>

    <template>
      <div class="c-wrapp">
        <div class="c-chat" ref="block">
          <Message
            v-for="m in messages"
            :key="m.text"
            :name="m.name"
            :text="m.text"
            :owner="m.id === user.id"
          />
        </div>
        <div class="c-form">
          <ChatForm />
        </div>
      </div>
    </template>
  </v-card>
</template>

<script>
import { mapState, mapMutations } from "vuex";
import Message from "@/components/Message";
import ChatForm from "@/components/ChatForm";
export default {
  middleware: ["chat"],
  head() {
    return {
      title: `Комната ${this.user.room}`
    };
  },
  components: { Message, ChatForm },
  computed: mapState(["user", "messages", "users"]),
  methods: {
    ...mapMutations(["clearData"]),
    exit() {
      this.$socket.emit("userLeft", this.user.id, () => {
        this.$router.push("/?message=leftChat");
        this.clearData();
      });
    }
  },
  data: () => ({
    drawer: true
  }),
  watch: {
    group() {
      this.drawer = false;
    },
    messages() {
      setTimeout(() => {
        if (undefined !== this.$refs.block)
          this.$refs.block.scrollTop = this.$refs.block.scrollHeight;
      });
    }
  }
};
</script>

<style scoped>
.c-wrpapp {
  height: 100%;
  position: relative;
  overflow: hidden;
}

.c-form {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 100px;
  background: #424242;
}

.c-chat {
  position: absolute;
  top: 65px;
  right: 0;
  left: 0;
  bottom: 100px;
  padding: 1rem;
  overflow-y: auto;
}
</style>
