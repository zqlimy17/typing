<template>

  <div>

    <div class="keyboard">

      <div class="keyboard__keys">

        <div
          v-for="elem in numberRow"
          :key="elem"
          class="keyboard__key"
          v-bind:class="{
            'keyboard__key--wide': wide.includes(elem),
            light: char === elem,
          }"
        >

          <span v-if="icons.includes(elem)">

            <i class="material-icons">{{ elem }}</i>

          </span>

          <span v-else v-bind:class="{ up: shift }"> {{ elem }} </span>

        </div>

      </div>

      <div class="keyboard__keys">

        <div
          v-for="elem in topRow"
          :key="elem"
          class="keyboard__key"
          v-bind:class="{
            'keyboard__key--wide': wide.includes(elem),
            light: char === elem || ((elem === 'left-shift' || elem === 'right-shift') && shift),
          }"
        >

          <span v-if="icons.includes(elem)">

            <i class="material-icons">{{ elem }}</i>

          </span>

          <span v-else v-bind:class="{ up: shift }"> {{ elem }} </span>

        </div>

      </div>

      <div class="keyboard__keys">

        <div
          v-for="elem in homeRow"
          :key="elem"
          class="keyboard__key"
          v-bind:class="{
            'keyboard__key--wide': wide.includes(elem),
            light: char === elem || ((elem === 'left-shift' || elem === 'right-shift') && shift),
          }"
        >

          <span v-if="elem === 'left-shift' || elem === 'right-shift'">

            <i class="material-icons">keyboard_arrow_up</i>

          </span>

          <span v-else-if="icons.includes(elem)">

            <i class="material-icons">{{ elem }}</i>

          </span>

          <span v-else v-bind:class="{ up: shift }"> {{ elem }} </span>

        </div>

      </div>

      <div class="keyboard__keys">

        <div
          v-for="elem in bottomRow"
          :key="elem"
          class="keyboard__key"
          v-bind:class="{
            'keyboard__key--wide': wide.includes(elem),
            light: char === elem || ((elem === 'left-shift' || elem === 'right-shift') && shift),
          }"
        >

          <span v-if="elem === 'left-shift' || elem === 'right-shift'">

            <i class="material-icons">keyboard_arrow_up</i>

          </span>

          <span v-else-if="elem === 'space'"> </span>

          <span v-else-if="icons.includes(elem)">

            <i class="material-icons">{{ elem }}</i>

          </span>

          <span v-else v-bind:class="{ up: shift }"> {{ elem }} </span>

        </div>

      </div>

      <div class="keyboard__keys">

        <div
          class="keyboard__key keyboard__key--wide"
          v-bind:class="{
            light: char === 'space',
          }"
        ></div>

      </div>

    </div>

  </div>

</template>

<script>
export default {
  name: "Keyboard",
  props: ["currentChar", "currentKeyboard"],
  data() {
    return {
      char: "",
      layout: "qwerty",
      shift: false,
      numberRow: ["`", "1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "-", "=", "keyboard_backspace"],
      topRow: ["keyboard_tab", "q", "w", "e", "r", "t", "y", "u", "i", "o", "p", "[", "]", "\\"],
      homeRow: ["keyboard_capslock", "a", "s", "d", "f", "g", "h", "j", "k", "l", ";", "'", "keyboard_return"],
      bottomRow: ["left-shift", "z", "x", "c", "v", "b", "n", "m", ",", ".", "?", "right-shift"],
      shiftKeys: ["~", "!", "@", "#", "$", "%", "^", "&", "*", "(", ")", "_", "+", ":", "{", "}", "|", '"', "<", ">", "?"],
      wide: ["keyboard_backspace", "keyboard_tab", "keyboard_capslock", "keyboard_return", "left-shift", "right-shift", "space"],
      icons: ["keyboard_backspace", "keyboard_tab", "keyboard_capslock", "keyboard_return", "left-shift", "right-shift", "space"],
      hidden: false,
    };
  },
  watch: {
    currentChar() {
      if (this.currentChar == this.currentChar.toUpperCase()) {
        if (this.shiftKeys.includes(this.currentChar)) {
          this.shift = false;
        } else {
          this.shift = true;
        }
      } else {
        this.shift = false;
      }
      switch (this.currentChar) {
        case "!":
          this.char = "1";
          break;
        case "_":
          this.char = "-";
          break;
        case ":":
          this.char = ";";
          break;
        case '"':
          this.char = "'";
          break;
        case "?":
          this.char = "/";
          break;
        case "(":
          this.char = "9";
          break;
        case ")":
          this.char = "0";
          break;
        case "+":
          this.char = "=";
          break;
        case "|":
          this.char = "\\";
          break;
        case "#":
          this.char = "3";
          break;
        case "@":
          this.char = "2";
          break;
        case "$":
          this.char = "4";
          break;
        case "%":
          this.char = "5";
          break;
        case "^":
          this.char = "6";
          break;
        case "&":
          this.char = "7";
          break;
        case "*":
          this.char = "8";
          break;
        case "{":
          this.char = "[";
          break;
        case "}":
          this.char = "]";
          break;
        case "<":
          this.char = ",";
          break;
        case ">":
          this.char = ".";
          break;
        case "~":
          this.char = "`";
          break;
        default:
          this.char = this.currentChar.toLowerCase();
      }
    },
    currentKeyboard() {
      switch (this.currentKeyboard) {
        case "colemak-dh":
          this.colemakDH();
          break;
        case "dvorak":
          this.dvorak();
          break;
        case "colemak":
          this.colemak();
          break;
        case "workman":
          this.workman();
          break;
        default:
          this.qwerty();
      }
    },
  },
  methods: {
    qwerty() {
      this.layout = "qwerty";
      this.numberRow = ["`", "1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "-", "=", "keyboard_backspace"];
      this.topRow = ["keyboard_tab", "q", "w", "e", "r", "t", "y", "u", "i", "o", "p", "[", "]", "\\"];
      this.homeRow = ["keyboard_capslock", "a", "s", "d", "f", "g", "h", "j", "k", "l", ";", "'", "keyboard_return"];
      this.bottomRow = ["left-shift", "z", "x", "c", "v", "b", "n", "m", ",", ".", "?", "right-shift"];
    },
    colemakDH() {
      this.layout = "colemakdh";
      this.numberRow = ["`", "1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "-", "=", "keyboard_backspace"];
      this.topRow = ["keyboard_tab", "q", "w", "f", "p", "b", "j", "l", "u", "y", ";", "[", "]", "\\"];
      this.homeRow = ["keyboard_capslock", "a", "r", "s", "t", "g", "k", "n", "e", "i", "o", "'", "keyboard_return"];
      this.bottomRow = ["left-shift", "z", "x", "c", "d", "v", "m", "h", ",", ".", "?", "right-shift"];
    },
    colemak() {
      this.layout = "colemak";
      this.numberRow = ["`", "1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "-", "=", "keyboard_backspace"];
      this.topRow = ["keyboard_tab", "q", "w", "f", "p", "g", "j", "l", "u", "y", ";", "[", "]", "\\"];
      this.homeRow = ["keyboard_capslock", "a", "r", "s", "t", "d", "h", "n", "e", "i", "o", "'", "keyboard_return"];
      this.bottomRow = ["left-shift", "z", "x", "c", "v", "b", "k", "m", ",", ".", "?", "right-shift"];
    },
    workman() {
      this.layout = "workman";
      this.numberRow = ["`", "1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "-", "=", "keyboard_backspace"];
      this.topRow = ["keyboard_tab", "q", "d", "r", "w", "b", "j", "f", "u", "p", ";", "[", "]", "\\"];
      this.homeRow = ["keyboard_capslock", "a", "s", "h", "t", "g", "y", "n", "e", "o", "i", "'", "keyboard_return"];
      this.bottomRow = ["left-shift", "z", "x", "m", "c", "v", "k", "l", ",", ".", "?", "right-shift"];
    },
    dvorak() {
      this.layout = "dvorak";
      this.numberRow = ["`", "1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "[", "]", "keyboard_backspace"];
      this.topRow = ["keyboard_tab", "'", ",", ".", "p", "y", "f", "g", "c", "r", "l", "/", "=", "\\"];
      this.homeRow = ["keyboard_capslock", "a", "o", "e", "u", "i", "d", "h", "t", "n", "s", "-", "keyboard_return"];
      this.bottomRow = ["left-shift", ";", "q", "j", "k", "x", "b", "m", "w", "v", "z", "right-shift"];
    },
  },
};
</script>

<style>
.keyboard {
  margin: 0 auto;
  color: var(--d-font);
  width: 728px;
}

.keyboard__keys {
  text-align: center;
  display: flex;
}

.keyboard__key {
  height: 45px;
  width: 45px;
  margin: 3px;
  border-radius: 4px;
  border: 1px solid var(--d-border);
  font-size: 1.05rem;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  vertical-align: middle;
  padding: 0;
  position: relative;
}

.keyboard__key--wide {
  flex-grow: 3;
}

.up {
  text-transform: uppercase;
}

.keyboard span {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  height: 24px;
}

.layouts {
  display: flex;
  width: 728px;
  margin: 0 auto;
}

.layouts button {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  flex-grow: 1;
  background: var(--d-bg);
  height: 45px;
  margin: 3px;
  padding: 0 1rem;
  border-radius: 4px;
  border: 1px solid var(--d-border);
  font-size: 1.05rem;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  vertical-align: middle;
  position: relative;
}

.light {
  background: var(--d-light) !important;
  z-index: 0;
}

.keyboard {
  animation: fadeInKeyboard ease 1.5s;
}

@keyframes fadeInKeyboard {
  0% {
    opacity: 0;
    transform: translateY(50px);
  }

  100% {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>

