<template>
  <div id="app">
    <main class="main">
      <h1>Simon Says</h1>
      <simon-game />
    </main>
  </div>
</template>

<script>
import "./assets/styles/main.scss";
import SimonGame from "./components/SimonGame.vue";
export default {
  name: "App",
  components: { SimonGame },
};
</script>

<style lang="scss">
h1 {
  margin-bottom: 20px;
}
</style>
