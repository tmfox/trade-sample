<template>
  <v-app>
    <v-main>
      <BitcoinForm></BitcoinForm>
    </v-main>
  </v-app>
</template>

<script>
import BitcoinForm from "./components/BitcoinForm";

export default {
  name: 'App',
  components: {BitcoinForm},

  data: () => ({
    //
  }),
};
</script>
