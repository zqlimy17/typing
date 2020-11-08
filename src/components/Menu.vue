<template>
<div>Menu</div>
<button v-on:click="newText">New Text</button>
</template>

<script>
export default {
    name: "Menu",
    props: {
        newText: {
            type: Function,
        },
    },
};
</script>

<style scoped></style>
