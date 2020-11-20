<template>
    <div class="post">
        <h2 class="title">{{ wpm }} WPM</h2>
        <h2 class="title">{{ (accuracy * 100).toFixed(2) }}% Accuracy</h2>
        <h2 class="title underline">Mistakes</h2>
        <div class="mistake" v-for="word in mistakes" :key="word.index">
            <p>{{ word }}</p>
        </div>
    </div>
</template>

<script>
export default {
    name: "Post",
    props: ["wpm", "mistakes", "accuracy"],
};
</script>

<style>
.post {
    width: 60%;
    margin: 1rem auto;
}

.title {
    text-align: center;
}

.underline {
    text-decoration: underline;
}

.post h2 {
    padding: 0.25rem 0;
}

.mistake {
    display: inline-block;
    width: 20%;
    text-align: left;
}
</style>
