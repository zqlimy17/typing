<template>
<button v-on:click="setTimer">timer</button>
{{ timeLimit }}
<button v-on:click="setCountdown">setCountdown</button>
{{ countdown }}
<Menu :newText="newText" :setCurrentTextStack="setCurrentTextStack" />
<Typing v-bind:currentText="currentText" />
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
            currentTextStack: [],
            currentWord: "",
            countdown: 0,
            timeLimit: 0,
            wpm: 0,
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
        newGame() {},
        setWpm() {},
        setCurrentTextStack() {
            console.log("Creating Text Stack");
            this.currentText !== "" ?
                (this.currentTextStack = this.currentText.split(" ")) :
                (this.currentTextStack = []);
            console.log(this.currentTextStack);
            console.log("Number of words: " + this.currentTextStack.length);
        },
        setCountdown() {
            this.countdown = 3;
            const that = this;
            let countdown = setInterval(function () {
                console.log(that.timeLimit);
                that.countdown -= 1;
                if (that.countdown <= 0) {
                    clearInterval(countdown);
                    that.setTimer();
                }
            }, 1000);
        },
        setTimer() {
            console.log("Timer Started");
            this.timeLimit = Math.ceil(this.currentText.length / 3);
            console.log("Time Limit:" + this.timeLimit);
            const that = this;
            let timer = setInterval(function () {
                console.log(that.timeLimit);
                that.timeLimit -= 1;
                if (that.timeLimit <= 0) {
                    clearInterval(timer);
                }
            }, 1000);
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
