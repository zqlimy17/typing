<template>
<div class="input">
    <input ref="userInput" tabindex="0" type="text" :disabled="!gameActive" :placeholder="textStack[0]" v-model="internalValue" />
</div>
</template>

<script>
export default {
    name: "Typing",
    props: ["gameActive", "textStack", "userInput"],
    computed: {
        internalValue: {
            get: function () {
                return this.value;
            },
            set: function (newValue) {
                this.$emit("inputchange", newValue);
            },
        },
    },
    watch: {
        gameActive: function () {
            if (this.gameActive === true) {
                const vm = this;
                vm.detailsEditable = true;
                vm.$nextTick(() => {
                    vm.$refs.userInput.focus();
                });
            }
        },
    },
};
</script>

<style>
.input {
    position: relative;
    display: flex;
    flex-direction: row;
    max-width: 400px;
    margin: 1rem auto;
    border-radius: 2px;
    padding: 1rem;
    border: 1px solid black;
    border-radius: 4px;
}

.input:after {
    content: "";
    position: absolute;
    left: 0px;
    right: 0px;
    bottom: 0px;
    z-index: 999;
    height: 2px;
    border-bottom-left-radius: 2px;
    border-bottom-right-radius: 2px;
}

.input input {
    display: block;
    margin: 0 auto;
    flex-grow: 1;
    color: black;
    font-size: 1.8rem;
    line-height: 2.4rem;
    vertical-align: middle;
    border-style: none;
    background: transparent;
    outline: none;
}

.input input:-webkit-input-placeholder {
    color: #7881a1;
}
</style>
