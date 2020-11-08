<template>
<div>Typing</div>
<div id="typing">
    <p>{{ currentText }}</p>
    <input type="text" :disabled="!gameActive" :placeholder="textStack[0]" v-model="internalValue" />
</div>
</template>

<script>
export default {
    name: "Typing",

    props: ["currentText", "gameActive", "textStack", "userInput"],
    computed: {
        internalValue: {
            get: function () {
                return this.value;
            },
            set: function (newValue) {
                this.$emit("inputchange", newValue);
            },
        },
    },
};
</script>

<style scoped></style>
