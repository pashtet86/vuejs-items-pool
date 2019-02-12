<template>
  <div id="app">
    <img width="15%" src="./assets/logo.png" />
    <h2>Simple mode</h2>
    <ItemsPool v-model="cars" name="cars" />
    <h2>Object mode, label</h2>
    <ItemsPool
      v-model="games"
      showKey="title"
      label="Best games ever"
      name="games"
    />
    <h2>Validation on</h2>
    <ItemsPool
      v-model="frameworks"
      name="frameworks"
      v-validate="{ required: true }"
    />
    <button class="btn" @click.prevent="validate">Submit/Validate</button>
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
      cars: ["Toyota", "Honda"],
      games: [
        { title: "Forza horizon", id: 1 },
        { title: "Detroit", id: 2 },
        { title: "GTA 5", id: 3 }
      ],
      frameworks: []
    };
  },
  methods: {
    validate() {
      this.$validator.validateAll().then(result => {
        if (result) {
          this.submit();
        } else {
          this.sendMessage();
        }
      });
    },
    submit() {
      console.log("submited");
    },
    sendMessage() {
      console.log("there are some errors");
    }
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

.btn {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  height: 36px;
  border-radius: 3px;
  font-size: 18px;
  font-weight: 600;
  padding: 0 20px;
  margin-left: 10px;
  text-align: center;
  background-color: white;
  color: #2d333d;
  border: solid 1px #e2e2e7;
  cursor: pointer;

  &[disabled] {
    cursor: not-allowed !important;
  }
}
</style>
