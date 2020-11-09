<template>
<div>
    <p>
        <span class="_success">
            {{ doneWords }}
        </span>
        <span class="_current _success"> </span>

        <span class="_blinker"></span>
        <span>{{ word }}</span>
        <span>
            {{ upcomingWords }}
        </span>
    </p>
</div>
</template>

<script>
export default {
    name: "Paragraph",
    props: ["doneWords", "currentWord", "upcomingWords"],
    data() {
        return {
            word: "",
            next: "",
            current: "",
            done: "",
        };
    },
    watch: {
        currentWord: function () {
            this.word = this.currentWord;
            console.log("Word is: " + this.word);
        },
    },
};
</script>

<style scoped>
._success {
    color: green;
}

._current {
    text-decoration: underline;
    text-decoration-skip-ink: auto;
}

._blinker {
    box-shadow: 0 0 1px 1px #888888;
    animation: blink-animation 1s steps(2, start) infinite;
}

@keyframes blink-animation {
    to {
        visibility: hidden;
    }
}
</style>
