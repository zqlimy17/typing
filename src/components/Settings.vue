<template>
<div class="settings" v-if="settings">
    <div class="inner">
        <h2>Settings</h2>
        <div class="sub">
            <h3>Theme</h3>
        </div>
        <div class="options" v-for="theme in themes" :key="theme.index" :class="{ active: theme === currentTheme }" v-on:click="updateTheme(theme)">
            {{ theme.toUpperCase() }}
        </div>

        <div class="sub">
            <h3>Keyboard Layout</h3>
        </div>
        <div class="options" v-for="keyboard in keyboards" :key="keyboard.index" :class="{ active: keyboard === currentKeyboard }" v-on:click="updateKeyboard(keyboard)">
            {{ keyboard.toUpperCase() }}
        </div>

        <p>
            This site is built with VueJS and it will use your browser's
            local storage to store your scores.
        </p>

        <div class="options active" style="margin: 2rem; width: 40%" v-on:click="handleSettings()">
            Save
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: "Settings",
    props: [
        "settings",
        "currentTheme",
        "currentKeyboard",
        "updateTheme",
        "updateKeyboard",
        "handleSettings",
    ],
    data() {
        return {
            keyboards: ["qwerty", "colemak", "colemak-dh", "workman", "dvorak"],
            themes: ["light", "dark", "sepia", "ocean", "forest"],
        };
    },
};
</script>

<style scoped>
.settings {
    position: absolute;
    top: 0;
    left: 0;
    height: 100vh;
    width: 100vw;
    background: var(--d-bg);

    color: var(--d-font);
    z-index: 2;
}

.inner {
    top: 50%;
    left: 50%;
    text-align: center;
    margin: 0 auto;
    position: relative;
    transform: translate(-50%, -50%);
}

.sub {
    padding: 1rem 0;
}

.active {
    background: var(--d-font);
    color: var(--d-bg);
    font-weight: 700;
}
</style>
