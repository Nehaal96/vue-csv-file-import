<template>
  <FileImport />
</template>

<script>
import FileImport from "./components/FileImport.vue";

export default {
  name: "App",
  components: {
    FileImport,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
