<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Logo da Empresa"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />
        
        <h2>Logo da Empresa</h2>
      </div>
    </v-app-bar>

    <v-main>
      <MemoryGame/>
    </v-main>
  </v-app>
</template>

<script>
import MemoryGame from './components/MemoryGame';

export default {
  name: 'App',

  components: {
    MemoryGame,
  },

  data: () => ({
    //
    methods: {
      resetGame() {
      location.reload()
    }
    },
  }),
};
</script>
