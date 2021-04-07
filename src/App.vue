<template>
  <v-app>
    <v-main>
      <bars />
    </v-main>
  </v-app>
</template>

<script>
import Bars from './components/Bars';

export default {
  name: 'App',

  components: {
    Bars,
  },

  data: () => ({
  }),
};
</script>
