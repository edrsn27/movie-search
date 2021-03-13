<template>
  <v-app id="inspire">
    <Header />

    <v-main class="grey lighten-3">
      <Nuxt />
    </v-main>
  </v-app>
</template>

<script>
import Header from "~/components/layouts/Header.vue";
export default {
  components: {
    Header
  }
};
</script>
