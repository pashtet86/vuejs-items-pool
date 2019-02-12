<template>
  <div id="app">
    <img width="15%" src="./assets/logo.png" />
    <h2>Default mode</h2>
    <ItemsPool v-model="items" />
    <h2>Object mode</h2>
    <ItemsPool v-model="objectsPool" showKey="title" />
  </div>
</template>

<script>
import ItemsPool from "./components/ItemsPool";

export default {
  name: "App",
  components: {
    ItemsPool
  },
  data() {
    return {
      items: ["Car", "Plane"],
      objectsPool: [{ title: "Toyota", id: 1 }, { title: "Honda", id: 1 }]
    };
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;

  align-items: center;

  img {
    flex-shrink: 0;
    margin-bottom: 30px;
  }
}
</style>
