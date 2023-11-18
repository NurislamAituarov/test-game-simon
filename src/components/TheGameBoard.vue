<template>
  <div class="game-board" :class="{ disabled: isDisabledBtn }">
    <div
      v-for="(color, index) in colors"
      :key="index"
      :class="`game-button ${color}-light`"
      @click="$emit('handle-button-click', index)"
    ></div>
  </div>
</template>

<script>
export default {
  name: "THeGameBoard",
  props: {
    colors: { type: Array, default: () => [] },
    isDisabledBtn: { type: Boolean },
  },
};
</script>

<style scoped lang="scss">
.game-board {
  position: relative;
  width: 302px;
  height: 295px;
  border-radius: 150px 150px 150px 150px;
  position: relative;
  box-shadow: 2px 1px 12px #aaa;
  .game-button {
    width: 100%;
    height: 100%;
    border-radius: 150px 150px 150px 150px;
    position: absolute;
    text-indent: 10000px;
    position: absolute;
    cursor: pointer;
    opacity: 0.6;
    &:hover {
      border: 2px solid black;
    }
  }

  .lit {
    opacity: 1;
  }

  .red-light {
    background-color: #ff5643;
    clip: rect(0px, 302px, 150px, 150px);
  }
  .blue-light {
    background-color: dodgerblue;
    clip: rect(0px, 150px, 150px, 0px);
  }
  .green-light {
    background-color: #bede15;
    clip: rect(150px, 150px, 300px, 0px);
  }
  .yellow-light {
    background-color: #feef33;
    clip: rect(150px, 302px, 300px, 150px);
  }
}

.disabled {
  pointer-events: none;
}
</style>