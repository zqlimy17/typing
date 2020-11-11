<template>
{{ timeLimit }}
{{ countdown }}
WPM = {{ wpm }}
<Menu :newGame="newGame" />
<Paragraph :doneWords="doneWords" :currentWord="currentWord" :upcomingWords="upcomingWords" :userInput="userInput" />
<Progress :doneWords="doneWords" :currentText="currentText" />
<Typing :gameActive="gameActive" :textStack="textStack" @inputchange="handleUserInput" />
<Keyboard />
</template>

<script>
import Keyboard from "./components/Keyboard.vue";
import Menu from "./components/Menu.vue";
import Paragraph from "./components/Paragraph.vue";
import Progress from "./components/Progress.vue";
import Typing from "./components/Typing.vue";

import allText from "./assets/text.json";

export default {
    name: "App",
    components: {
        Keyboard,
        Menu,
        Paragraph,
        Progress,
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
            gameActive: false,
            doneWords: "",
            currentWord: "",
            upcomingWords: "",
        };
    },
    mounted() {
        console.log("App Mounted");
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
            console.log(this.textStack);
            console.log("Number of words: " + this.textStack.length);
        },
        setCountdown() {
            this.countdown = 1;
            this.currentWord = this.textStack[0];
            this.upcomingWords = this.textStack.slice(1).join(" ");
            const that = this;
            let countdown = setInterval(function () {
                console.log(that.countdown);
                that.countdown -= 1;
                if (that.countdown <= 0) {
                    clearInterval(countdown);
                    that.setTimer();
                }
            }, 1000);
        },
        setTimer() {
            this.gameActive = true;
            this.timeLimit = Math.ceil(this.currentText.length / 2.5);

            const that = this;
            let timer = setInterval(function () {
                if (that.timeLimit <= 0) {
                    clearInterval(timer);
                    that.gameActive = false;
                }
                that.timeElapsed += 1;
                that.timeLimit -= 1;
            }, 1000);
        },
        handleUserInput(newVal) {
            this.userInput = newVal;
        },
        reset() {
            this.textStack = [];
            this.userInput= "";
            this.timeElapsed= 0;
            this.wpm= 0;
            this.gameActive= false;
            this.doneWords= "";
            this.currentWord= "";
            this.upcomingWords= "";
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
            console.log('THIS DONEWORDS LENGTH: ' + this.doneWords);
            this.wpm = Math.ceil((this.doneWords.length * 12) / this.timeElapsed );
        }
    },
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}
</style>
