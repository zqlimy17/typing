<template>
<div>
    <h1 class="text-center p-3">Typing</h1>
    <Timer :countdown="countdown" :timeLimit="timeLimit" :timeElapsed="timeElapsed" />
    <Paragraph :doneWords="doneWords" :currentWord="currentWord" :upcomingWords="upcomingWords" :userInput="userInput" @char="currentChar = $event" />
    <Progress :doneWords="doneWords" :currentText="currentText" :currentTheme="currentTheme" />
    <div v-bind:class="{
                wrong: currentChar === 'keyboard_backspace',
                hidden: gameActive === false && gameStarted === false,
            }">
        <Typing :gameActive="gameActive" :textStack="textStack" @inputchange="userInput = $event" />
        <Keyboard :currentChar="currentChar" :currentKeyboard="currentKeyboard" />
    </div>
    <Menu :newGame="newGame" :retry="retry" :handleSettings="handleSettings" :gameStarted="gameStarted" :postGame="postGame" :wpm="wpm" />
    <Settings :settings="settings" :currentTheme="currentTheme" :currentKeyboard="currentKeyboard" :updateTheme="updateTheme" :updateKeyboard="updateKeyboard" :handleSettings="handleSettings" />
</div>
</template>

<script>
import Keyboard from "./components/Keyboard.vue";
import Menu from "./components/Menu.vue";
import Paragraph from "./components/Paragraph.vue";
import Progress from "./components/Progress.vue";
import Settings from "./components/Settings.vue";
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
        Settings,
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
            postGame: false,
            doneWords: "",
            currentWord: "",
            upcomingWords: "",
            currentChar: "",
            settings: false,
            currentTheme: "dark",
            currentKeyboard: "colemak-dh",
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
        async retry() {
            this.reset();
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
            this.postGame = false;
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
                if (that.timeLimit <= 0 || !that.gameActive) {
                    clearInterval(timer);
                    that.gameActive = false;
                    that.gameStarted = false;
                    that.postGame = true;
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
            this.gameStarted = false;
            this.doneWords = "";
            this.currentWord = "";
            this.upcomingWords = "";
        },
        updateTheme(val) {
            this.currentTheme = val;
        },
        updateKeyboard(val) {
            this.currentKeyboard = val;
            localStorage.setItem("keyboard", val);
        },
        handleSettings() {
            this.settings = !this.settings;
        },
    },

    watch: {
        userInput: {
            handler() {
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
        gameActive() {
            if (this.gameActive === false) this.timeLimit = 0;
        },
        doneWords() {
            this.wpm = Math.ceil(
                (this.doneWords.length * 12) / this.timeElapsed
            );
        },
        currentTheme() {
            let htmlElement = document.documentElement;
            if (this.currentTheme === "dark") {
                localStorage.setItem("theme", "dark");
                htmlElement.setAttribute("theme", "dark");
            } else {
                localStorage.setItem("theme", "light");
                htmlElement.setAttribute("theme", "light");
            }
        },
    },
    mounted() {
        let htmlElement = document.documentElement;
        let _theme = localStorage.getItem("theme");
        let _keyboard = localStorage.getItem("keyboard");
        if (_keyboard) this.currentKeyboard = _keyboard;
        if (_theme === "dark") {
            htmlElement.setAttribute("theme", "dark");
            this.currentTheme = "dark";
        } else {
            htmlElement.setAttribute("theme", "light");
            this.currentTheme = "light";
        }
    },
};
</script>

<style>
:root {
    --app-background-color: white;
    --dynamic-font-color: black;
    --dynamic-border-color: black;
    --dynamic-light-color: greenyellow;
}

[theme="dark"] {
    --app-background-color: black;
    --dynamic-font-color: white;
    --dynamic-border-color: white;
    --dynamic-light-color: green;
}

*,
*:before,
*:after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "Merriweather", serif;
}

#app {
    height: 100vh;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    background: var(--app-background-color);
    color: var(--dynamic-font-color);
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
