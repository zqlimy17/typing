<template>
<div class="menu">
    <div class="post" v-if="gameActive && gameStarted">
        <button class="options" v-on:click="retry">RESTART</button>
    </div>

    <div v-if="!gameStarted || postGame">
        <button v-on:click="newGame" class="options">
            <i class="material-icons">sports_esports</i> NEW
        </button>
        <div class="ongoing" v-if="!gameActive && postGame">
            <button class="options" v-on:click="retry">
                RETRY THIS TEXT
            </button>
        </div>
        <button v-on:click="handleSettings" class="settings-button">
            <i class="material-icons">settings</i> 
        </button>
    </div>
</div>
</template>

<script>
export default {
    name: "Menu",
    props: [
        "newGame",
        "retry",
        "handleSettings",
        "gameStarted",
        "postGame",
        "gameActive",
    ],
};
</script>

<style>
.menu {
    width: 30%;
    margin: 0 auto;
    text-align: center;
}

.menu .options {
    width: 100%;
}

.settings-button {
    color: var(--d-font);
    cursor: pointer;
    background: var(--d-bg);
    border: none;
    font-size: 1.05rem;;
    position: absolute;
    top: 0;
    right: 0;
    margin: 2rem;
}
.settings-button:not(.active):hover {
    color: gold;
}

.hidden {
    display: none;
}
</style>
