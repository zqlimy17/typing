<template>
<div>
    <p>
        <span class="_success">
            {{ doneWords }}
        </span>
        <span class="_current _success"> </span>

        <span class="_blinker"> </span>
        <span> </span>
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
            word: this.currentWord,
        };
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
    border-left: 1px solid black;
    animation: blink-animation 1s steps(2, start) infinite;
}

@keyframes blink-animation {
    to {
        visibility: hidden;
    }
}
</style>
