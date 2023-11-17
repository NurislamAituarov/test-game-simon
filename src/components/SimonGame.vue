<template>
  <div id="simon-game">
    <div class="game-board" :class="{ disabled: !sequence.length }">
      <div
        v-for="(color, index) in colors"
        :key="index"
        :class="`game-button ${color}-light`"
        @click="handleButtonClick(index)"
      ></div>
    </div>
    <div class="controls">
      <base-btn :is-playing="isPlaying" @start-game="startGame" />

      <p v-if="!isPlaying && gameOver">Game Over!</p>
    </div>
  </div>
</template>
  
  <script>
import redSound from "@/assets/audio/sounds_1.mp3";
import greenSound from "@/assets/audio/sounds_2.mp3";
import blueSound from "@/assets/audio/sounds_3.mp3";
import yellowSound from "@/assets/audio/sounds_4.mp3";
import BaseBtn from "./base/BaseBtn.vue";

const audioFiles = {
  red: redSound,
  green: greenSound,
  blue: blueSound,
  yellow: yellowSound,
};

export default {
  components: { BaseBtn },
  data() {
    return {
      sequence: [],
      userSequence: [],
      isPlaying: false,
      gameOver: false,
      colors: ["red", "green", "blue", "yellow"],
      difficulty: {
        easy: 1500,
        medium: 1000,
        hard: 400,
      },
    };
  },
  methods: {
    startGame() {
      this.sequence = [];
      this.gameOver = false;
      this.playSequence();
    },
    playSequence() {
      this.isPlaying = true;
      const delay = this.difficulty.hard;

      this.sequence.push(this.colors[Math.floor(Math.random() * 4)]);
      this.lightUpSequence(delay).then(() => {
        this.isPlaying = false;
      });
    },
    lightUpSequence(delay) {
      return this.sequence.reduce((promise, color, index) => {
        return promise.then(() => {
          return new Promise((resolve) => {
            setTimeout(() => {
              this.lightUpButton(color);
              this.playSound(color);
              setTimeout(() => {
                this.clearButton(color);
                resolve();
              }, delay);
            }, delay * index * 2);
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
      if (this.isPlaying) return;

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
        this.userSequence = [];
        this.sequence = [];
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
  gap: 50px;
}

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
  