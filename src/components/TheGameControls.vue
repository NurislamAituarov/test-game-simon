<template>
  <div class="controls">
    <h2 class="title">Round: {{ roundsNumber }}</h2>
    <base-btn :is-playing="isPlaying" @start-game="$emit('start-game')" />

    <p v-if="!isPlaying && gameOver">
      Sorry, you lost after {{ sequence.length }} rounds!
    </p>

    <div class="game-difficulty">
      <h2 class="title">Game difficulty</h2>

      <div
        class="game-difficulty__item"
        v-for="name of difficultyKeys"
        :key="name"
      >
        <input
          v-model="selectedDifficulty"
          type="radio"
          name="gameDifficulty"
          :id="name"
          :value="name"
          @change="$emit('select-game-difficulty', name)"
        />
        <label :for="name">{{ name }}</label>
      </div>
    </div>
  </div>
</template>

<script>
import { difficulty } from "@/lib/constants";
import BaseBtn from "./base/BaseBtn.vue";

export default {
  components: { BaseBtn },
  name: "TheGameControls",
  props: {
    roundsNumber: { type: Number },
    isPlaying: { type: Boolean },
    gameOver: { type: Boolean },
    sequence: { type: Array, default: () => [] },
    value: { type: String },
  },

  data() {
    return {
      selectedDifficulty: "hard",
    };
  },

  computed: {
    difficultyKeys() {
      return Object.keys(difficulty);
    },
  },
};
</script>


<style scoped lang="scss">
.controls {
  width: 200px;
}

.game-difficulty {
  display: flex;
  flex-direction: column;
  gap: 5px;
  margin-top: 30px;
  .title {
    margin: 0;
  }
  &__item {
    display: flex;
    align-items: center;
    gap: 5px;
    input,
    label {
      cursor: pointer;
    }
  }
}

.title {
  margin: 20px 0;
}
</style>