<template>
    <div class="progress-bar">
        <div class="progress" :style="`width: ${progress()}%`" role="prrogressbar"
            :aria-valuenow="progress()"
            aria-valuemin="0" aria-valuemax="100">
            <img src="../assets/duck.png">
        </div>
    </div>
</template>

<script>
export default {
    name: "Progress",
    props: ["doneWords", "currentText"],
    methods: {
        progress() {
            return Math.ceil(this.doneWords.length / this.currentText.length * 100)
        }
    },
    watch: {
        doneWords: function () {
            this.progress();
        }
    }
}
</script>

<style scoped>
.progress-bar {
    height: 20px;
    width: 100%;
    background-color: grey;
}
.progress{
    height: 20px;
    background-color: #fbd41b;

}

img {
    height: 50px;
}
</style>