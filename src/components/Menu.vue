<template>
<div>Menu</div>
<button v-on:click="newText">New Text</button>
<button v-on:click="setCurrentTextStack">setCurrentTextStack</button>
</template>

<script>
export default {
    name: "Menu",
    props: {
        newText: {
            type: Function,
        },
        setCurrentTextStack: {
            type: Function,
        },
    },
};
</script>

<style scoped></style>
