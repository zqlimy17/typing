<template>
<div class="progress-bar">
    <div class="progress" :style="`width: ${progress()}%`">
        <img src="../assets/duck0.png" v-if="duck % 3 === 0" />
        <img src="../assets/duck1.png" v-else-if="duck % 3 === 1" />
        <img src="../assets/duck2.png" v-else-if="duck % 3 === 2" />
        <img src="../assets/duck0.png" v-else style="float: right" />
    </div>
</div>
</template>

<script>
export default {
    name: "Progress",
    props: ["doneWords", "currentText"],
    data() {
        return {
            duck: 0,
        };
    },
    methods: {
        progress() {
            return Math.ceil(
                (this.doneWords.length / this.currentText.length) * 100
            );
        },
        updateDuck() {
            this.duck += 1;
        },
    },
    watch: {
        doneWords: function () {
            this.updateDuck();
        },
    },
};
</script>

<style scoped>
.progress-bar {
    margin-right: 50px;
    margin: 0 auto;
    max-width: 60%;
}

.progress {
    margin: 2rem;
    height: 2px;
    border: dashed 2px darkgoldenrod;
}

img {
    top: -25px;
    height: 50px;
    position: relative;
    left: 100%;
}
</style>
