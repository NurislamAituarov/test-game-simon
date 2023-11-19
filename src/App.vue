<template>
  <div id="app">
    <main class="main">
      <h1>Simon Says</h1>
      <simon-game />
    </main>
  </div>
</template>

<script>
import SimonGame from "./components/TheSimonGame.vue";
export default {
  name: "App",
  components: { SimonGame },
};
</script>

<style lang="scss">
h1 {
  margin-bottom: 60px;
  text-align: center;
}

@media all and (max-width: 768px) {
  h1 {
    margin-bottom: 30px;
  }
}
</style>
