<template>
  <v-app>
    <v-main>
      <ImageConverter />
    </v-main>
  </v-app>
</template>

<script>
import ImageConverter from './components/ImageConverter.vue';

export default {
  name: 'App',

  components: {
    ImageConverter
  },

  data: () => ({
    //
  }),
};
</script>
