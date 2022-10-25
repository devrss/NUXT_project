<template>
  <v-app>
    <v-app-bar absolute extended app dark>
      <v-toolbar-title>Soft-skill</v-toolbar-title>
      <template v-slot:extension>
        <v-btn
          class="ma-3"
          color="primary"
          rounded
          outlined
          link
          :to="{ name: 'index' }"
          exact
          >Задания</v-btn
        >
        <v-btn
          class="ma-3"
          color="primary"
          rounded
          outlined
          link
          :to="{ name: 'article' }"
          >Статьи</v-btn
        >

        <v-dialog v-model="dialog" persistent max-width="600px">
          <template v-slot:activator="{ on }">
            <v-btn class="ma-3" color="primary" rounded outlined v-on="on"
              >чат</v-btn
            >
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">Вход в систему</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-form ref="form" v-model="valid" lazy-validation>
                  <v-row>
                    <v-col cols="12">
                      <v-text-field
                        v-model="name"
                        :counter="16"
                        :rules="nameRules"
                        label="Имя"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-text-field
                        label="E-mail"
                        v-model="email"
                        :rules="emailRules"
                        value="example"
                        suffix="@gmail.com"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-text-field
                        v-model="room"
                        :rules="roomRules"
                        label="Введите комнату"
                        required
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-form>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="error" class="mr-4" @click="dialog = false"
                >Закрыть</v-btn
              >
              <v-btn
                color="primary"
                class="mr-4"
                :disabled="!valid"
                @click="submit"
                v-on:click="dialog = false"
                >Войти</v-btn
              >
            </v-card-actions>
          </v-card>
        </v-dialog>
      </template>
    </v-app-bar>
    <v-main app>
      <v-container fluid>
        <nuxt />
      </v-container>
    </v-main>
    <v-footer app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
import { mapMutations } from "vuex";
export default {
  head: {
    title: "Добро пожаловать!",
  },
  sockets: {
    connect: function () {
      console.log("socket connected");
    },
  },
  data: () => ({
    dialog: false,
    valid: true,
    name: "",
    nameRules: [
      (v) => !!v || "Введите имя",
      (v) => (v && v.length <= 16) || "Имя не должно превышать 16 символов",
    ],
    email: "",
    emailRules: [
      (v) => !!v || "E-mail is required",
      (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
    ],
    room: "",
    roomRules: [(v) => !!v || "Введите комнату"],
  }),
  methods: {
    ...mapMutations(["setUser"]),
    submit() {
      if (this.$refs.form.validate()) {
        const user = {
          name: this.name,
          room: this.room,
        };

        this.$socket.emit("userJoined", user, (data) => {
          if (typeof data === "string") {
            console.error(data);
          } else {
            user.id = data.userId;
            this.setUser(user);
            this.$router.push("/chat");
          }
        });
      }
    },
  },
};
</script>
