<template>
<button v-on:click="setTimer">timer</button>
{{ timeLimit }}
<button v-on:click="setCountdown">setCountdown</button>
{{ countdown }}
<Menu :newGame="newGame" />
<Paragraph :doneWords="doneWords" :currentWord="currentWord" :upcomingWords="upcomingWords" :userInput="userInput" />
<Typing :gameActive="gameActive" :textStack="textStack" @inputchange="handleUserInput" />
</template>

<script>
import Menu from "./components/Menu.vue";
import Paragraph from "./components/Paragraph.vue";
import Typing from "./components/Typing.vue";
import allText from "./text.json";

export default {
    name: "App",
    components: {
        Menu,
        Paragraph,
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
            wpm: 0,
            field: "",
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
            this.timeLimit = Math.ceil(this.currentText.length * 100);

            const that = this;
            let timer = setInterval(function () {
                that.timeLimit -= 1;
                if (that.timeLimit <= 0) {
                    clearInterval(timer);
                    that.gameActive = false;
                }
            }, 1000);
        },
        handleUserInput(newVal) {
            this.userInput = newVal;
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
