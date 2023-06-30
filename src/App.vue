<template>
  <img alt="Vue logo" src="./assets/logo.png" class="logo">
  <div>
    <WhatsappForm />
  </div>
</template>

<script>
import WhatsappForm from './views/WhatsappForm.vue'

export default {
  name: 'App',
  components: {
    WhatsappForm,
  },
}
</script>

<style>
.logo {
  margin-top: 20px;
  width: 10.5em;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-image: url('./assets/bg.svg');
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
  background-attachment: fixed;
  min-height: 100vh;
}
/* Estilos para WebKit (Safari y Chrome) */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background-color: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 4px;
}
</style>
