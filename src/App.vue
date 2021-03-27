<template>
  <v-app>
    <v-main>
      <div> <img alt="Vue logo" src="./assets/logo.png">
      <IBusqueda/>
      </div>
     
    </v-main>
  </v-app>
</template>

<script>

import IBusqueda from './components/IBusqueda'

export default {
  name: 'App',

  components: {
    IBusqueda
  },

  data: () => ({
    //
  }),
};
</script>
