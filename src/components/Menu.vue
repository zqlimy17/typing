<template>
<div>Menu</div>
<button v-on:click="newText">New Text</button>
<button v-on:click="splitSentence">splitSentence</button>
</template>

<script>
export default {
    name: "Menu",
    props: {
        newText: {
            type: Function,
        },
        splitSentence: {
            type: Function,
        },
    },
};
</script>

<style scoped></style>
