<template>
<div>Menu</div>
<button v-on:click="newGame">New Text</button>
</template>

<script>
export default {
    name: "Menu",
    props: {
        newGame: {
            type: Function,
        },
    },
};
</script>

<style scoped></style>
