<template>
<div>Typing</div>
<div id="typing">
    <input type="text" :disabled="!gameActive" :placeholder="textStack[0]" v-model="internalValue" />
</div>
</template>

<script>
export default {
    name: "Typing",
    props: ["gameActive", "textStack", "userInput"],
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
