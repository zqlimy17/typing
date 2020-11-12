<template>
<div id="keyboard">
    Current Char: {{ currentChar }}<br />
    Char: {{ char }} <br />
    Shift: {{ shift }}
    <h1>Keyboard</h1>
    <div class="keyboard">
        <div class="keyboard__keys">
            <template v-for="elem in numberRow" :key="elem">
                <div class="keyboard__key" v-bind:class="{
                            'keyboard__key--wide': wide.includes(elem),
                            light: char === elem,
                        }">
                    <span v-if="icons.includes(elem)">
                        <i class="material-icons">{{ elem }}</i>
                    </span>
                    <span v-else>
                        {{ elem }}
                    </span>
                </div>
            </template>
        </div>
        <div class="keyboard__keys">
            <template v-for="elem in topRow" :key="elem">
                <div class="keyboard__key" v-bind:class="{
                            'keyboard__key--wide': wide.includes(elem),
                            light:
                                char === elem ||
                                ((elem === 'left-shift' ||
                                    elem === 'right-shift') &&
                                    shift),
                        }">
                    <span v-if="icons.includes(elem)">
                        <i class="material-icons">{{ elem }}</i>
                    </span>
                    <span v-else>
                        {{ elem }}
                    </span>
                </div>
            </template>
        </div>

        <div class="keyboard__keys">
            <template v-for="elem in homeRow" :key="elem">
                <div class="keyboard__key" v-bind:class="{
                            'keyboard__key--wide': wide.includes(elem),
                            light:
                                char === elem ||
                                ((elem === 'left-shift' ||
                                    elem === 'right-shift') &&
                                    shift),
                        }">
                    <span v-if="
                                elem === 'left-shift' || elem === 'right-shift'
                            ">
                        <i class="material-icons">keyboard_arrow_up</i>
                    </span>
                    <span v-else-if="icons.includes(elem)">
                        <i class="material-icons">{{ elem }}</i>
                    </span>
                    <span v-else>
                        {{ elem }}
                    </span>
                </div>
            </template>
        </div>

        <div class="keyboard__keys">
            <template v-for="elem in bottomRow" :key="elem">
                <div class="keyboard__key" v-bind:class="{
                            'keyboard__key--wide': wide.includes(elem),
                            light:
                                char === elem ||
                                ((elem === 'left-shift' ||
                                    elem === 'right-shift') &&
                                    shift),
                        }">
                    <span v-if="
                                elem === 'left-shift' || elem === 'right-shift'
                            ">
                        <i class="material-icons">keyboard_arrow_up</i>
                    </span>
                    <span v-else-if="elem === 'space'"> </span>
                    <span v-else-if="icons.includes(elem)">
                        <i class="material-icons">{{ elem }}</i>
                    </span>
                    <span v-else>
                        {{ elem }}
                    </span>
                </div>
            </template>
        </div>
        <div class="keyboard__keys">
            <div class="keyboard__key keyboard__key--wide" v-bind:class="{
                        light: char === 'space',
                    }"></div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: "Keyboard",
    props: ["currentChar"],
    data() {
        return {
            char: "",
            shift: false,
            numberRow: [
                "`",
                "1",
                "2",
                "3",
                "4",
                "5",
                "6",
                "7",
                "8",
                "9",
                "0",
                "-",
                "=",
                "keyboard_backspace",
            ],
            topRow: [
                "keyboard_tab",
                "q",
                "w",
                "e",
                "r",
                "t",
                "y",
                "u",
                "i",
                "o",
                "p",
                "[",
                "]",
                "\\",
            ],
            homeRow: [
                "keyboard_capslock",
                "a",
                "s",
                "d",
                "f",
                "g",
                "h",
                "j",
                "k",
                "l",
                ";",
                "'",
                "keyboard_return",
            ],
            bottomRow: [
                "left-shift",
                "z",
                "x",
                "c",
                "v",
                "b",
                "n",
                "m",
                ",",
                ".",
                "?",
                "right-shift",
            ],
            shiftKeys: [
                "~",
                "!",
                "@",
                "#",
                "$",
                "%",
                "^",
                "&",
                "*",
                "(",
                ")",
                "_",
                "+",
                ":",
                "{",
                "}",
                "|",
                '"',
                "<",
                ">",
                "?",
            ],

            wide: [
                "keyboard_backspace",
                "keyboard_tab",
                "keyboard_capslock",
                "keyboard_return",
                "left-shift",
                "right-shift",
                "space",
            ],
            icons: [
                "keyboard_backspace",
                "keyboard_tab",
                "keyboard_capslock",
                "keyboard_return",
                "left-shift",
                "right-shift",
                "space",
            ],
            hidden: false,
        };
    },
    watch: {
        currentChar: function () {
            if (
                this.currentChar == this.currentChar.toUpperCase() ||
                this.shiftKeys.includes(this.currentChar)
            ) {
                this.shift = true;
            } else {
                this.shift = false;
            }
            switch (this.currentChar) {
                case "!":
                    this.char = "1";
                    break;
                case "_":
                    this.char = "-";
                    break;
                case ":":
                    this.char = ";";
                    break;
                case '"':
                    this.char = "'";
                    break;
                case "?":
                    this.char = "/";
                    break;
                case "(":
                    this.char = "9";
                    break;
                case ")":
                    this.char = "0";
                    break;
                case "+":
                    this.char = "=";
                    break;
                case "|":
                    this.char = "\\";
                    break;
                case "#":
                    this.char = "3";
                    break;
                case "@":
                    this.char = "2";
                    break;
                case "$":
                    this.char = "4";
                    break;
                case "%":
                    this.char = "5";
                    break;
                case "^":
                    this.char = "6";
                    break;
                case "&":
                    this.char = "7";
                    break;
                case "*":
                    this.char = "8";
                    break;
                case "{":
                    this.char = "[";
                    break;
                case "}":
                    this.char = "]";
                    break;
                case "<":
                    this.char = ",";
                    break;
                case ">":
                    this.char = ".";
                    break;
                case "~":
                    this.char = "`";
                    break;
                default:
                    this.char = this.currentChar.toLowerCase();
            }
        },
    },
};
</script>

<style>
.keyboard {
    position: fixed;
    left: 0;
    bottom: 0;
    width: 728px;
    padding: 5px 0;
}

.keyboard__keys {
    text-align: center;
    display: flex;
}

.keyboard__key {
    height: 45px;
    width: 45px;
    margin: 3px;
    border-radius: 4px;
    border: 1px solid black;
    font-size: 1.05rem;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    vertical-align: top;
    padding: 0;
    position: relative;
}

.light {
    background: greenyellow;
}

.keyboard__key--wide {
    flex-grow: 3;
}
</style>
