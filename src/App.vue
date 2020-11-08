<template>
<Menu :newText="newText" :splitSentence="splitSentence" />
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
            currentTextSplit: [],
            currentWord: "",
            currentTime: 0,
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
            console.log(this.currentText);
        },
        newGame() {},
        wpm() {},
        splitSentence() {
            console.log("Splitting Text");
            this.currentText !== "" ?
                (this.currentTextSplit = this.currentText.split(" ")) :
                (this.currentTextSplit = []);
            console.log(this.currentTextSplit);
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
