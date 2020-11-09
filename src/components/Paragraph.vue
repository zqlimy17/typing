<template>
<div>
    <p>
        <span class="_success">
            {{ doneWords }}
        </span>
        <span class="_current _success">{{ done }}</span>
        <span class="_current _danger">{{ wrongDone }}</span>
        <span class="_current" v-bind:class="{ _blinker: blinker }"></span>
        <span class="_current">{{ upcoming }}</span>
        <span v-if="!wrongInput">&nbsp;</span>
        <span>{{ upcomingWords }} </span>
    </p>
</div>
</template>

<script>
export default {
    name: "Paragraph",
    props: ["doneWords", "currentWord", "upcomingWords", "userInput"],
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
        currentWord: function () {
            console.log("this current word is: " + this.currentWord);
            if (this.currentWord) {
                if (this.currentWord[this.currentWord.length - 1] === " ") {
                    this.word = this.currentWord.slice(0, -1);
                } else {
                    this.word = this.currentWord;
                }
                this.currentStack = this.word.split("");
                this.done = "";
                this.upcoming = this.word;
            } else if (this.currentWord === undefined) {
                console.log("Clearing");
                this.upcoming = "";
                this.done = "";
            }
        },
        userInput: function () {
            let toCompare = this.word.slice(0, this.userInput.length);
            console.log(this.upcoming, this.wrongDone);
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
                this.wrongDone = this.wrongDone.substr(
                    0,
                    this.wrongDone.length - 1
                );
            } else if (this.userInput.length > toCompare.length) {
                this.wrongDone = this.wrongDone.concat(" ");
                this.wrongInput = true;
            } else {
                let nextLetter = this.upcoming[0];
                if (nextLetter !== undefined) {
                    this.wrongDone = this.wrongDone.concat(nextLetter);
                    this.upcoming = this.upcoming.substr(1);
                }
            }
        },
    },
};
</script>

<style scoped>
._success {
    color: #99cc00;
}

._danger {
    background: lightcoral;
}

._current {
    text-decoration: underline;
    text-decoration-skip-ink: auto;
}

._blinker {
    box-shadow: 0 0 1px 1px black;
    animation: blink-animation 1s steps(2, start) infinite;
    animation-delay: 2s;
}

@keyframes blink-animation {
    to {
        visibility: hidden;
    }
}
</style>
