<template>
  <div id="app">
    <PasswordGenerator/>
  </div>
</template>

<script>
import PasswordGenerator from './components/PasswordGenerator';

export default {
  name: 'App',
  components: {
    PasswordGenerator,
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
