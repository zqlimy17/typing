<template>
<div>
    <h1 class="text-center p-3">Typing</h1>
    <Timer :countdown="countdown" :timeLimit="timeLimit" :wpm="wpm" />
    <Paragraph :doneWords="doneWords" :currentWord="currentWord" :upcomingWords="upcomingWords" :userInput="userInput" @char="currentChar = $event" :handleMistake="handleMistake" :mistakes="mistakes" :handleWrong="handleWrong" />
    <Progress :doneWords="doneWords" :currentText="currentText" :currentTheme="currentTheme" />
    <div v-bind:class="{
                wrong: currentChar === 'keyboard_backspace',
                hidden:
                    (gameActive === false && gameStarted === false) ||
                    postGame === true,
            }">
        <Typing :gameActive="gameActive" :textStack="textStack" @inputchange="userInput = $event" />
        <Keyboard :currentChar="currentChar" :currentKeyboard="currentKeyboard" />
    </div>
    <Post v-if="postGame" :wpm="wpm" :mistakes="mistakes" :accuracy="accuracy" />
    <Menu :newGame="newGame" :retry="retry" :handleSettings="handleSettings" :gameStarted="gameStarted" :postGame="postGame" :wpm="wpm" :gameActive="gameActive" />
    <Settings :settings="settings" :currentTheme="currentTheme" :currentKeyboard="currentKeyboard" :updateTheme="updateTheme" :updateKeyboard="updateKeyboard" :handleSettings="handleSettings" />
</div>
</template>

<script>
import Keyboard from "./components/Keyboard.vue";
import Menu from "./components/Menu.vue";
import Paragraph from "./components/Paragraph.vue";
import Post from "./components/Post.vue";
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
        Post,
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
            mistakes: [],
            wrongCount: 0,
            accuracy: 0,
        };
    },
    methods: {
        newText() {
            this.currentText =
                allText[Math.floor(Math.random() * allText.length)];
        },
        async newGame() {
            this.textStack = [];
            this.userInput = "";
            this.timeElapsed = 0;
            this.wpm = 0;
            this.wrongCount = 0;
            this.accuracy = 0;
            this.gameActive = false;
            this.postGame = false;
            this.gameStarted = false;
            this.doneWords = "";
            this.currentWord = "";
            this.upcomingWords = "";
            await this.newText();
            await this.setTextStack();
            await this.setCountdown();
        },
        async retry() {
            this.textStack = [];
            this.userInput = "";
            this.timeElapsed = 0;
            this.wpm = 0;
            this.wrongCount = 0;
            this.accuracy = 0;
            this.gameActive = false;
            this.postGame = false;
            this.doneWords = "";
            this.currentWord = "";
            this.upcomingWords = "";
            await this.setTextStack();
            await this.setCountdown();
        },
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
            this.timeLimit = Math.ceil(this.currentText.length * 200);
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
                    that.gameActive = false;
                    clearInterval(timer);
                } else {
                    that.timeLimit -= 1;
                    that.timeElapsed += 1;
                }
            }, 1000);
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
        handleMistake() {
            let last = this.mistakes[this.mistakes.length - 1];
            if (this.currentWord !== last) {
                this.mistakes.push(this.currentWord);
            }
        },
        handleWrong() {
            this.wrongCount++;
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
            if (this.timeLimit === 0 && this.gameActive === false) {
                this.postGame = true;
            }
        },
        doneWords() {
            this.wpm =
                Math.ceil((this.doneWords.length * 12) / this.timeElapsed) || 0;
        },
        currentTheme() {
            let htmlElement = document.documentElement;
            if (this.currentTheme === "dark") {
                localStorage.setItem("theme", "dark");
                htmlElement.setAttribute("theme", "dark");
            } else if (this.currentTheme === "sepia") {
                localStorage.setItem("theme", "sepia");
                htmlElement.setAttribute("theme", "sepia");
            } else if (this.currentTheme === "ocean") {
                localStorage.setItem("theme", "ocean");
                htmlElement.setAttribute("theme", "ocean");
            } else if (this.currentTheme === "forest") {
                localStorage.setItem("theme", "forest");
                htmlElement.setAttribute("theme", "forest");
            } else {
                localStorage.setItem("theme", "light");
                htmlElement.setAttribute("theme", "light");
            }
        },
        gameStarted() {
            if (this.gameStarted) this.postGame = false;
        },
        wrongCount() {
            this.accuracy = 1 - this.wrongCount / this.currentText.length;
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
        } else if (_theme === "sepia") {
            htmlElement.setAttribute("theme", "sepia");
            this.currentTheme = "sepia";
        } else if (_theme === "ocean") {
            htmlElement.setAttribute("theme", "ocean");
            this.currentTheme = "ocean";
        } else if (_theme === "forest") {
            htmlElement.setAttribute("theme", "forest");
            this.currentTheme = "forest";
        } else {
            htmlElement.setAttribute("theme", "light");
            this.currentTheme = "light";
        }
    },
};
</script>

<style>
:root {
    --d-bg: whitesmoke;
    --d-font: black;
    --d-border: black;
    --d-light: greenyellow;
    --d-success: green;
    --d-placeholder: #7881a1;
}

[theme="dark"] {
    --d-bg: #303030;
    --d-font: white;
    --d-border: white;
    --d-light: green;
    --d-success: greenyellow;
    --d-placeholder: #7881a1;
}

[theme="sepia"] {
    --d-bg: #704214;
    --d-font: #d4b595;
    --d-border: #eadbcb;
    --d-light: green;
    --d-success: greenyellow;
    --d-placeholder: darkgrey;
}

[theme="ocean"] {
    --d-bg: #002535;
    --d-font: #bfd2d9;
    --d-border: #3b89ac;
    --d-light: green;
    --d-success: greenyellow;
    --d-placeholder: #22646e;
}

[theme="forest"] {
    --d-bg: #5c6b24;
    --d-font: #e7e8a6;
    --d-border: #cdcd8e;
    --d-light: green;
    --d-success: greenyellow;
    --d-placeholder: #a8b59e;
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
    background: var(--d-bg);
    color: var(--d-font);
}

.text-center {
    text-align: center;
}

.p-3 {
    padding: 1rem;
}

.wrong .input {
    background-color: rgba(255, 0, 0, 0.4);
    border-color: red;
}

.hidden {
    display: none;
}

.options {
    color: var(--d-font);
    cursor: pointer;
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
    margin-bottom: 1rem;
}

.options:not(.active):hover {
    background: gold;
    color: black;
}
</style>
