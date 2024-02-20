<template>
  <div id="app">
    <PropuestaUno :date="date" @update:fecha="printFechaUno" />

  </div>
</template>

<script>
import PropuestaUno from './components/PropuestaUno.vue'

export default {
  name: 'App',
  components: {
    PropuestaUno,
  },
  data() {
    return {
      //Ejemplo de como pasar la fecha guardada
      date: {
        day: '20',
        month: '05',
        year: '1998'
      }
    };
  },
  methods: {
    printFechaUno(fecha) {
      console.log('Fecha 1: ', fecha);
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: grid;
  justify-content: center;
  align-items: center;
}
</style>
