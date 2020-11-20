<template>
    <div class="input">
        <input
            ref="userInput"
            tabindex="0"
            type="text"
            :disabled="!gameActive"
            :placeholder="textStack[0]"
            v-model="internalValue"
        />
    </div>
</template>

<script>
export default {
    name: "Typing",
    props: ["gameActive", "textStack", "userInput"],
    computed: {
        internalValue: {
            get() {
                return this.userInput;
            },
            set(newValue) {
                this.$emit("inputchange", newValue);
            },
        },
    },
    watch: {
        gameActive() {
            if (this.gameActive === true) {
                const vm = this;
                vm.detailsEditable = true;
                vm.$nextTick(() => {
                    vm.$refs.userInput.focus();
                });
            }
        },
        userInput() {},
    },
};
</script>

<style>
.input {
    background: var(--d-bg);
    color: var(--d-font);
    position: relative;
    display: flex;
    flex-direction: row;
    max-width: 400px;
    margin: 1rem auto;
    border-radius: 2px;
    padding: 1rem;
    border: 1px solid var(--d-border);
    border-radius: 4px;
    animation: fadeInTyping ease 1.5s;
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
    color: var(--d-font);
    font-size: 1.8rem;
    line-height: 2.4rem;
    vertical-align: middle;
    border-style: none;
    background: transparent;
    outline: none;
}

::placeholder {
    color: var(--d-placeholder);
}

@keyframes fadeInTyping {
    0% {
        opacity: 0;
    }

    30% {
        opacity: 0;
    }

    100% {
        opacity: 1;
    }
}
</style>
