<template>
  <div id="simon-game">
    <the-game-board
      :colors="colors"
      :isDisabledBtn="isDisabledBtn"
      @handle-button-click="handleButtonClick"
    />

    <the-game-controls
      :rounds-number="roundsNumber"
      :is-playing="isPlaying"
      :game-over="gameOver"
      :sequence="sequence"
      @start-game="startGame"
      @select-game-difficulty="selectGameDifficulty"
    />
  </div>
</template>
  
  <script>
import { difficulty, audioFiles } from "@/lib/constants";
import TheGameBoard from "./TheGameBoard.vue";
import TheGameControls from "./TheGameControls.vue";

export default {
  components: { TheGameBoard, TheGameControls },
  data() {
    return {
      sequence: [],
      userSequence: [],
      isPlaying: false,
      gameOver: false,
      colors: ["red", "green", "blue", "yellow"],
      delay: 400,
      isDisabledBtn: true,
      numberBtnPresses: 0,
    };
  },

  computed: {
    roundsNumber() {
      return this.gameOver ? 0 : this.sequence.length;
    },

    difficultyKeys() {
      return Object.keys(difficulty);
    },
  },

  methods: {
    startGame() {
      this.numberBtnPresses = 0;
      this.sequence = [];
      this.gameOver = false;
      this.isDisabledBtn = false;
      this.playSequence();
    },
    playSequence() {
      this.isPlaying = true;
      const delay = this.delay;

      this.sequence.push(this.colors[Math.floor(Math.random() * 4)]);
      this.lightUpSequence(delay).then(() => {
        this.isPlaying = false;
        this.numberBtnPresses = 0;
      });
    },

    selectGameDifficulty(name) {
      this.delay = difficulty[name];
    },

    lightUpSequence(delay) {
      return this.sequence.reduce((promise, color) => {
        return promise.then(() => {
          return new Promise((resolve) => {
            setTimeout(() => {
              this.lightUpButton(color);
              this.playSound(color);
              setTimeout(() => {
                this.clearButton(color);
                resolve();
              }, delay);
            }, delay);
          });
        });
      }, Promise.resolve());
    },

    lightUpButton(color) {
      const button = document.querySelector(`.${color}-light`);
      button.classList.add("lit");
    },
    clearButton(color) {
      const button = document.querySelector(`.${color}-light`);
      button.classList.remove("lit");
    },
    playSound(color) {
      const audio = new Audio(audioFiles[color]);
      audio.play();
    },

    handleButtonClick(index) {
      this.numberBtnPresses++;
      if (this.isPlaying || this.numberBtnPresses > this.sequence.length)
        return;

      const clickedColor = this.colors[index];
      this.lightUpButton(clickedColor);
      this.playSound(clickedColor);
      setTimeout(() => {
        this.clearButton(clickedColor);
      }, 500);

      this.userSequence.push(clickedColor);

      const isEqual = this.userSequence.every(
        (color, index) => color === this.sequence[index]
      );

      if (!isEqual) {
        this.gameOver = true;
        this.isDisabledBtn = true;
        this.userSequence = [];
      } else if (this.userSequence.length === this.sequence.length) {
        this.userSequence = [];
        setTimeout(() => {
          this.playSequence();
        }, 1000);
      }
    },
  },
};
</script>
  

<style scoped lang="scss">
#simon-game {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 50px;
}
</style>
  