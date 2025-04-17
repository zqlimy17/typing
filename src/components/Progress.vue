<template>

  <div class="progress-bar">

    <div class="progress" :style="`width: ${progress()}%`">

      <img
        src="../assets/duck0.png"
        v-if="duck % 3 === 0"
        :class="{
          forest: currentTheme === 'forest',
          ocean: currentTheme === 'ocean',
          invert: currentTheme === 'dark',
          sepia: currentTheme === 'sepia',
        }"
      />

      <img
        src="../assets/duck1.png"
        v-else-if="duck % 3 === 1"
        :class="{
          forest: currentTheme === 'forest',
          ocean: currentTheme === 'ocean',
          invert: currentTheme === 'dark',
          sepia: currentTheme === 'sepia',
        }"
      />

      <img
        src="../assets/duck2.png"
        v-else-if="duck % 3 === 2"
        :class="{ forest: currentTheme === 'forest', ocean: currentTheme === 'ocean', invert: currentTheme === 'dark', sepia: currentTheme === 'sepia' }"
      />

      <img
        src="../assets/duck0.png"
        v-else
        style="float: right"
        :class="{
          forest: currentTheme === 'forest',
          ocean: currentTheme === 'ocean',
          invert: currentTheme === 'dark',
          sepia: currentTheme === 'sepia',
        }"
      />

    </div>

  </div>

</template>

<script>
export default {
  name: "Progress",
  props: ["doneWords", "currentText", "currentTheme"],
  data() {
    return {
      duck: 0,
    };
  },
  methods: {
    progress() {
      return Math.ceil((this.doneWords.length / this.currentText.length) * 100);
    },
    updateDuck() {
      this.duck += 1;
    },
  },
  watch: {
    doneWords() {
      this.updateDuck();
    },
  },
};
</script>

<style scoped>
.progress-bar {
  background: var(--d-bg);
  margin-right: 50px;
  margin: 0 auto;
  color: var(--d-font);
  width: 60%;
  max-width: 750px;
}

.progress {
  margin: 2rem 0;
  height: 2px;
  border: solid 1px var(--d-border);
}

img {
  top: -25px;
  height: 50px;
  position: relative;
  left: 100%;
}

.invert {
  filter: invert(100%);
}

.sepia {
  filter: invert(100%);
  opacity: 0.5;
}

.forest {
  filter: invert(100%) sepia(1) hue-rotate(45deg) saturate(1);
}

.ocean {
  filter: invert(100%) sepia(1) hue-rotate(135deg) saturate(2);
}
</style>

