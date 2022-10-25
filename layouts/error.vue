<template>
  <v-card class="mx-auto" max-width="1200">
    <div cols="12" class="container">
      <h1 v-if="error.statusCode === 404">Страница не найдена</h1>
      <h1 v-else>Произошла ошибка</h1>
      <nuxt-link to="/">Вернуться на главную</nuxt-link>
    </div>
  </v-card>
</template>

<script>
export default {
  props: ["error"]
};
</script>
