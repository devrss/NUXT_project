<template>
  <div>
    <v-alert
      v-model="alert"
      border="left"
      close-text="Close Alert"
      color="primary"
      dark
      elevation="4"
      dismissible
    >
      <v-row align="center">
        <v-col
          class="grow"
        >Составьте план развития на три месяца, полгода и год. Запишите: цели, компетенции и навыки на каждом из этапов.</v-col>
        <v-col class="shrink">
          <v-btn color="#5599DE" rounded>Выполнить</v-btn>
        </v-col>
      </v-row>
    </v-alert>
    <div class="text-center">
      <v-btn v-if="!alert" color="deep-purple accent-4" rounded dark @click="alert = true">Reset</v-btn>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      alert: true
    };
  }
};
</script>
