<template>
<h1 class="text-center p-3">Typing</h1>
<Timer :countdown="countdown" :timeLimit="timeLimit" :timeElapsed="timeElapsed" />
<Paragraph :doneWords="doneWords" :currentWord="currentWord" :upcomingWords="upcomingWords" :userInput="userInput" @char="currentChar = $event" />
<Progress :doneWords="doneWords" :currentText="currentText" />
<div v-bind:class="{
            wrong: currentChar === 'keyboard_backspace',
            hidden: gameActive === false && gameStarted === false,
        }">
    <Typing :gameActive="gameActive" :textStack="textStack" @inputchange="userInput = $event" />
    <Keyboard :currentChar="currentChar" />
</div>
<Menu :newGame="newGame" />
</template>

<script>
import Keyboard from "./components/Keyboard.vue";
import Menu from "./components/Menu.vue";
import Paragraph from "./components/Paragraph.vue";
import Progress from "./components/Progress.vue";
import Timer from "./components/Timer.vue";
import Typing from "./components/Typing.vue";

import allText from "./assets/text.json";

export default {
    name: "App",
    components: {
        Keyboard,
        Menu,
        Paragraph,
        Progress,
        Timer,
        Typing,
    },
    data() {
        return {
            allText,
            currentText: "",
            textStack: [],
            userInput: "",
            countdown: 0,
            timeLimit: 0,
            timeElapsed: 0,
            wpm: 0,
            gameStarted: false,
            gameActive: false,
            doneWords: "",
            currentWord: "",
            upcomingWords: "",
            currentChar: "",
        };
    },
    methods: {
        newText() {
            this.currentText =
                allText[Math.floor(Math.random() * allText.length)];
        },
        async newGame() {
            this.reset();
            await this.newText();
            await this.setTextStack();
            await this.setCountdown();
        },
        setWpm() {},
        setTextStack() {
            this.currentText !== "" ?
                (this.textStack = this.currentText
                    .split(/(\S+\s+)/)
                    .filter(function (n) {
                        return n;
                    })) :
                (this.textStack = []);
        },
        setCountdown() {
            this.gameStarted = true;
            this.countdown = 5;
            this.timeLimit = Math.ceil(this.currentText.length / 2.5);

            this.currentWord = this.textStack[0];
            this.upcomingWords = this.textStack.slice(1).join(" ");
            const that = this;
            let countdown = setInterval(function () {
                that.countdown -= 1;
                if (that.countdown <= 0) {
                    clearInterval(countdown);
                    that.setTimer();
                }
            }, 900);
        },
        setTimer() {
            this.gameActive = true;

            const that = this;
            let timer = setInterval(function () {
                if (that.timeLimit <= 0) {
                    clearInterval(timer);
                    that.gameActive = false;
                    that.gameStarted = false;
                }
                that.timeElapsed += 1;
                that.timeLimit -= 1;
            }, 1000);
        },
        reset() {
            this.textStack = [];
            this.userInput = "";
            this.timeElapsed = 0;
            this.wpm = 0;
            this.gameActive = false;
            this.doneWords = "";
            this.currentWord = "";
            this.upcomingWords = "";
        },
    },
    watch: {
        userInput: {
            handler: function () {
                if (this.userInput === this.textStack[0]) {
                    this.userInput = "";
                    this.doneWords = this.doneWords.concat(this.textStack[0]);

                    this.textStack.shift();

                    this.currentWord = this.textStack[0];
                    this.upcomingWords = this.textStack.slice(1).join(" ");
                    if (this.textStack.length <= 0) {
                        this.gameActive = false;
                    }
                }
            },
            immediate: true,
        },
        gameActive: function () {
            if (this.gameActive === false) this.timeLimit = 0;
        },
        doneWords: function () {
            this.wpm = Math.ceil(
                (this.doneWords.length * 12) / this.timeElapsed
            );
        },
    },
};
</script>

<style>
*,
*:before,
*:after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "Merriweather", serif;
}

#app {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

.text-center {
    text-align: center;
}

.p-3 {
    padding: 1rem;
}

.wrong .input {
    background-color: lightcoral;
    border-color: red;
}

.hidden {
    display: none;
}
</style>
