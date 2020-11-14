<template>
<div class="menu">
    <div class="post">
        <button class="options" v-on:click="retry">Retry</button>
    </div>
    <div :class="{ hidden: gameStarted }">
        <button v-on:click="newGame" class="options">
            <i class="material-icons">sports_esports</i> Play
        </button>
        <button v-on:click="handleSettings" class="options">
            <i class="material-icons">settings</i> Settings
        </button>
    </div>
</div>
</template>

<script>
export default {
    name: "Menu",
    props: ["newGame", "retry", "handleSettings", "gameStarted", "postGame"],
};
</script>

<style>
.menu {
    width: 60%;
    margin: 0 auto;
    text-align: center;
}

.options {
    color: var(--dynamic-font-color);
    cursor: pointer;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    flex-grow: 1;
    background: var(--app-background-color);
    height: 45px;
    margin: 3px;
    padding: 0 1rem;
    border-radius: 4px;
    border: 1px solid var(--dynamic-border-color);
    font-size: 1.05rem;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    vertical-align: middle;
    position: relative;
    margin-bottom: 1rem;
}

.hidden {
    display: none;
}
</style>
