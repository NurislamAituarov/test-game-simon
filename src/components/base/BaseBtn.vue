<template>
  <button @click="$emit('start-game')" :disabled="isPlaying" class="btn">
    Start
  </button>
</template>

<script>
export default {
  name: "BaseBtn",
  props: {
    isPlaying: { type: Boolean },
  },
};
</script>


<style scoped lang="scss">
.btn {
  width: 112px;
  font-size: 22px;
  border-radius: 10px 10px 10px 10px;
  -webkit-border-radius: 10px 10px 10px 10px;
  background: #6dabe8;
  border: none;
  padding: 4px;
  cursor: pointer;
  transition: all 0.5s;
  &:hover {
    background: #78bcff;
  }
}
</style>