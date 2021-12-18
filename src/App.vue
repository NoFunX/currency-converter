<template>
  <v-app>
    <Main/>
  </v-app>
</template>

<script>
  import Main from '@/pages/Main.vue'

  export default {
    name: 'App',

    data: () => ({
      //
    }),

    components: {
      Main
    }
  };
</script>

<style lang="scss" scoped>
  .v-application {
    background-color: #3b5b80 !important;
  }
</style>
