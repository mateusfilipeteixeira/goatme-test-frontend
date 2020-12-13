<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <v-btn
        elevation="2"
        large
        @click="registrationMode = true"
      >
        Adicionar evento
      </v-btn>
    </v-app-bar>

    <v-main>
      <TheRegistrationForm
        :registrationMode="registrationMode"
        @closeModal="registrationMode = false"
      />
      <TheCalendar />
    </v-main>
  </v-app>
</template>

<script>
import TheCalendar from './components/TheCalendar.vue';
import TheRegistrationForm from './components/TheRegistrationForm.vue';

export default {
  name: 'App',

  components: {
    TheCalendar,
    TheRegistrationForm
  },

  data: () => ({
    registrationMode: false
  }),
};
</script>

<style>
.v-toolbar__content {
  justify-content: flex-end;
}
</style>