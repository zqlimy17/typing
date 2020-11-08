<template>
<div>Typing</div>
<div id="typing">
    <p>{{ currentText }}</p>
    <input type="text" />
</div>
</template>

<script>
export default {
    name: "Typing",
    props: ["currentText"],
    mounted() {},
};
</script>

<style scoped></style>
