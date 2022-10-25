<template>
  <v-container>
    <v-form>
      <v-row>
        <v-col cols="12">
          <v-textarea
            placeholder="Введите сообщение"
            type="text"
            clear-icon="mdi-close-circle"
            clearable
            filled
            outlined
            no-resize
            rows="1"
            v-model="text"
            append-outer-icon="mdi-send"
            @keydown.enter="send"
            @click:append-outer="send"
          />
        </v-col>
      </v-row>
    </v-form>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    text: ""
  }),
  methods: {
    send() {
      this.$socket.emit(
        "createMessage",
        {
          text: this.text,
          id: this.$store.state.user.id
        },
        data => {
          if (typeof data === "string") {
            console.error(data);
          } else {
            this.text = "";
          }
        }
      );
    }
  }
};
</script>
