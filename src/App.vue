<template>
  <v-app>
    <v-content class="mt-1">
      <Meteo/>
    </v-content>
  </v-app>
</template>

<script>
import Meteo from "./components/Meteo";

export default {
  name: 'App',

  components: {
    Meteo,
  },

  data: () => ({
    //
  }),
};
</script>
