<template>
<div class="post">
    <h2>WPM: {{ wpm }}</h2>
</div>
</template>

<script>
export default {
    name: "Post",
    props: ["wpm", "mistakes"],
};
</script>

<style>
.post {
    width: 60%;
    margin: 1rem auto;
    text-align: center;
}
</style>
