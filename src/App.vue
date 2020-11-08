<template>
<button v-on:click="setTimer">timer</button>
{{ timeLimit }}
<button v-on:click="setCountdown">setCountdown</button>
{{ countdown }}
<Menu :newGame="newGame" />
<Typing :currentText="currentText" :gameActive="gameActive" :textStack="textStack" @inputchange="handleUserInput" />
</template>

<script>
import Menu from "./components/Menu.vue";
import Typing from "./components/Typing.vue";
import allText from "./text.json";

export default {
    name: "App",
    components: {
        Menu,
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
        };
    },
    mounted() {
        console.log("App Mounted");
    },
    methods: {
        newText() {
            console.log("New Text Generated");
            this.currentText =
                allText[Math.floor(Math.random() * allText.length)];
            console.log("Number of characters: " + this.currentText.length);
        },
        async newGame() {
            await this.newText();
            await this.setTextStack();
            await this.setCountdown();
        },
        setWpm() {},
        setTextStack() {
            console.log("Creating Text Stack");
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
            this.countdown = 3;
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
            console.log("Timer Started");
            this.gameActive = true;
            this.timeLimit = Math.ceil(this.currentText.length / 3);
            console.log("Time Limit:" + this.timeLimit);
            const that = this;
            let timer = setInterval(function () {
                console.log(that.userInput);
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
                console.log(this.userInput);
                console.log(this.textStack[0]);
                if (this.userInput === this.textStack[0]) {
                    this.userInput = "";
                    this.textStack.shift();
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
