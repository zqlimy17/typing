<template>
<div class="stats">
    <div class="stat">
        <h3>Best WPM</h3>
        <p>{{ bestWPM }}</p>
    </div>
    <div class="stat">
        <h3>Average WPM (Last 10)</h3>
        <p>{{ lastTenWPM.toFixed(0) }}</p>
    </div>
    <div class="stat">
        <h3>Latest WPM</h3>
        <p>{{ lastWPM }}</p>
    </div>
    <div class="stat">
        <h3>Text Typed</h3>
        <p>{{ scores.length }}</p>
    </div>
</div>
</template>

<script>
export default {
    name: "Statistics",
    props: ["bestWPM", "scores", "lastWPM", "wpm", "lastTenWPM"],
};
</script>

<style>
.stats {
    width: 60%;
    margin: 0 auto;
    text-align: center;
}

.stat {
    width: 25%;
    display: inline-block;
}

.stat h3 {
    text-decoration: underline;
}
</style>
