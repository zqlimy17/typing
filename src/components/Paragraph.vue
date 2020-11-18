<template>
  <div id="paragraph">
    <div :class="{ running: currentLayout === 'running' }">
      <span class="_success" id="correct">
        {{ doneWords }}
      </span>
      <span class="_current _success" id="currentCorrect">{{ done }}</span>
      <span class="_current _danger" id="currentWrong">{{ wrongDone }}</span>
      <span class="_current" v-bind:class="{ _blinker: blinker }"></span>
      <span class="_current">{{ upcoming }}</span>
      <span v-if="!wrongInput" style="display: inline-block">&nbsp;</span>
      <span>{{ upcomingWords }} </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Paragraph",
  props: [
    "gameStarted",
    "doneWords",
    "currentWord",
    "upcomingWords",
    "userInput",
    "mistakes",
    "handleMistake",
    "handleWrong",
    "currentLayout",
  ],
  data() {
    return {
      word: "",
      upcoming: "",
      done: "",
      doneStack: [],
      currentStack: [],
      wrongInput: false,
      wrongDone: "",
      userStack: [],
      blinker: true,
    };
  },
  watch: {
    gameStarted() {
      this.shiftText();
    },
    currentWord() {
      if (this.currentWord) {
        if (this.currentWord[this.currentWord.length - 1] === " ") {
          this.word = this.currentWord.slice(0, -1);
        } else {
          this.word = this.currentWord;
        }
        this.currentStack = this.word.split("");
        this.done = "";
        this.upcoming = this.word;
        this.emitChar(this.upcoming[0]);
      } else if (this.currentWord === undefined) {
        this.upcoming = "";
        this.done = "";
      }
    },
    async userInput() {
      let toCompare = await this.word.slice(0, this.userInput.length);
      this.blinker = false;
      setTimeout(() => {
        this.blinker = true;
      }, 1);
      if (this.userInput === toCompare) {
        this.wrongInput = false;
        if (this.userInput.length > this.done.length) {
          let nextLetter = this.upcoming[0];
          this.done = this.done.concat(nextLetter);
        }
        this.upcoming = this.word.substr(toCompare.length);

        this.emitChar(this.upcoming[0] || "space");

        this.wrongDone = "";
        this.done = this.done.slice(0, toCompare.length);
      } else if (
        this.userInput.length <
        this.done.length + this.wrongDone.length
      ) {
        if (
          this.wrongDone.length === 1 ||
          this.userInput.length === toCompare.length
        ) {
          this.wrongInput = false;
        }
        this.upcoming = this.word.substr(toCompare.length);
        this.emitChar("keyboard_backspace");

        this.wrongDone = this.wrongDone.substr(0, this.wrongDone.length - 1);
      } else if (this.userInput.length > toCompare.length) {
        this.wrongDone = this.wrongDone.concat(" ");
        this.wrongInput = true;
        this.handleMistake();
        this.emitChar("keyboard_backspace");
        this.handleWrong();
      } else {
        this.emitChar("keyboard_backspace");
        if (this.wrongDone.length === 0) this.handleWrong();
        this.handleMistake();
        let nextLetter = this.upcoming[0];
        if (nextLetter !== undefined) {
          this.wrongDone = this.wrongDone.concat(nextLetter);
          this.upcoming = this.upcoming.substr(1);
        }
      }
      if (this.currentWord === undefined) {
        this.upcoming = "";
      }
      if (this.currentLayout === "running") {
        this.shiftText();
      }
    },
  },
  methods: {
    emitChar(v) {
      this.$emit("char", v);
    },
    async shiftText() {
      let correct = await document.querySelector("#correct").offsetWidth;
      let currentCorrect = await document.querySelector("#currentCorrect")
        .offsetWidth;
      let currentWrong = await document.querySelector("#currentWrong")
        .offsetWidth;
      document.querySelector("#correct").style.marginLeft = `${-(
        correct +
        currentCorrect +
        currentWrong
      )}px`;
    },
  },
};
</script>

<style scoped>
#paragraph {
  width: 60%;
  max-width: 900px;
  color: var(--d-font);
  margin: 0 auto;
}

#paragraph span {
  font-size: 1.5rem;
}

.running {
  white-space: nowrap;
  padding-left: 50%;
}

._success {
  color: var(--d-success);
}

._danger {
  background-color: rgba(255, 0, 0, 0.4);
}

._current {
  text-decoration: underline;
  text-decoration-skip-ink: auto;
}

._blinker {
  box-shadow: 0 0 1px 1px var(--d-font);
  animation: blink-animation 1s steps(2, start) infinite;
  animation-delay: 2s;
}

@keyframes blink-animation {
  to {
    visibility: hidden;
  }
}
</style>
