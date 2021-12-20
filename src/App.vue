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
    background: url('D:/учеба/converter/src/assets/Depositphotos_60335027_l-2015-pic905-895x505-34408.jpg') !important;
    background-size: cover !important;
  }
</style>
