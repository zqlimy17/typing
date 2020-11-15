<template>
<div class="menu">
    <div class="post" v-if="postGame">
        <button class="options" v-on:click="retry">RETRY</button>
    </div>
    <div class="ongoing" v-if="gameActive">
        <button class="options" v-on:click="retry">RESTART</button>
    </div>
    <div v-if="!gameStarted">
        <button v-on:click="newGame" class="options">
            <i class="material-icons">sports_esports</i> PLAY
        </button>
        <button v-on:click="handleSettings" class="options">
            <i class="material-icons">settings</i> SETTINGS
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

.hidden {
    display: none;
}
</style>
