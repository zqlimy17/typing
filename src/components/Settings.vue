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
        <br />
        <div class=" options active" style="margin: 2rem 0 5rem 0; width: 40%; background-color: gold; color: black" v-on:click="handleSettings()">
            Save
        </div>

        <p>
            This site uses your browser's local storage to store your
            scores.
        </p>
        <div class="options clear" style="width: 40%" v-on:click="open">
            CLEAR DATA
        </div>
    </div>
    <div v-if="confirmClear" class="confirm">
        <div class="inner">
            <div class="options w-100" v-on:click="close()">BACK</div>
            <div class="options w-100" id="final_confirm" v-on:click="clear()">
                CONFIRM
            </div>
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
        "forceRerender",
    ],
    data() {
        return {
            keyboards: ["qwerty", "colemak", "colemak-dh", "workman", "dvorak"],
            themes: ["light", "dark", "sepia", "ocean", "forest"],
            confirmClear: false,
        };
    },
    methods: {
        clear() {
            localStorage.clear();
            this.confirmClear = false;
            this.forceRerender();
        },
        open() {
            this.confirmClear = true;
        },
        close() {
            this.confirmClear = false;
        },
    },
};
</script>

<style scoped>
.settings,
.confirm {
    position: absolute;
    top: 0;
    left: 0;
    height: 100vh;
    width: 100vw;
    background: var(--d-bg);
    color: var(--d-font);
}

.settings {
    z-index: 2;
}

.confirm {
    z-index: 3;
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

.clear:hover,
#final_confirm:hover {
    background: red !important;
    color: white !important;
}

.active {
    background: var(--d-font);
    color: var(--d-bg);
    font-weight: 700;
}

.w-100 {
    width: 50%;
}
</style>
