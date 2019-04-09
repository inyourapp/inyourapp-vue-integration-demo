<template>
  <div id="app">
    <HelloText/>
    <HelloSuggestions/>
  </div>
</template>

<script>
import HelloText from './components/HelloText';
import HelloSuggestions from './components/HelloSuggestions';

export default {
  name: 'App',
  components: {
    HelloText,
    HelloSuggestions,
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
