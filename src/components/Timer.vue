<template>
<div class="timer">
    <div class="countdown" v-if="countdown > 0">
        <div class="_colored-blocks">
            <div class="_colored-blocks-rotater">
                <div class="_colored-block"></div>
                <div class="_colored-block"></div>
                <div class="_colored-block"></div>
            </div>
            <div class="_colored-blocks-inner"></div>
            <div class="_text_ready">READY</div>
            <div class="_text_go">START</div>
        </div>
        <div class="_inner">
            <svg class="_numbers" viewBox="0 0 100 100">
                <defs>
                    <path class="_num-path-1" d="M40,28 55,22 55,78" />
                    <path class="_num-join-1-2" d="M55,78 55,83 a17,17 0 1,0 34,0 a20,10 0 0,0 -20,-10" />
                    <path class="_num-path-2" d="M69,73 l-35,0 l30,-30 a16,16 0 0,0 -22.6,-22.6 l-7,7" />
                    <path class="_num-join-2-3" d="M28,69 Q25,44 34.4,27.4" />
                    <path class="_num-path-3" d="M30,20 60,20 40,50 a18,15 0 1,1 -12,19" />
                </defs>
                <path class="_numbers-path" d="M-10,20 60,20 40,50 a18,15 0 1,1 -12,19 
               Q25,44 34.4,27.4
               l7,-7 a16,16 0 0,1 22.6,22.6 l-30,30 l35,0 L69,73 
               a20,10 0 0,1 20,10 a17,17 0 0,1 -34,0 L55,83 
               l0,-61 L40,28" />
            </svg>
        </div>
    </div>
    <div class="timeLimit" v-if="timeLimit > 0">
        Time Limit: {{ timeLimit }}s
        <span style="float:right">WPM: {{ wpm }}</span>
    </div>
    <div v-else>
        &nbsp;
    </div>
</div>
</template>

<script>
export default {
    name: "Timer",
    props: ["countdown", "timeLimit", "wpm"],
};
</script>

<style>
.timer {
    width: 60%;
    margin: 0 auto;
    padding-bottom: 10px;
}

.countdown {
    opacity: 0;
    z-index: 1;
    animation: fade 5s;
    position: absolute;
    left: 50%;
    top: 50%;
    width: 500px;
    height: 140px;
    margin-top: -70px;
    padding: 10px;
    border-radius: 20px;
    transform: translateX(-50%);
}

._colored-blocks {
    overflow: hidden;
    position: absolute;
    left: 50%;
    top: 0;
    width: 500px;
    height: 100%;
    margin-left: -250px;
    padding: 10px;
    border-radius: 20px;
    perspective: 1000px;
    animation: CountdownAnim 4s ease-in-out infinite, contAnim 4s;
}

._colored-blocks-rotater {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    border-radius: inherit;
    animation: rotation 1.3s linear infinite;
}

._colored-blocks-inner {
    overflow: hidden;
    position: relative;
    height: 100%;
    background: grey;
    border-radius: inherit;
}

._colored-block {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 300%;
    height: 300%;
    transform-origin: 0 0;
}

._colored-block:nth-child(1) {
    transform: rotate(0deg) skewX(-30deg);
    background-color: lightcoral;
}

._colored-block:nth-child(2) {
    transform: rotate(120deg) skewX(-30deg);
    background-color: greenyellow;
}

._colored-block:nth-child(3) {
    transform: rotate(240deg) skewX(-30deg);
    background-color: gold;
}

._inner {
    overflow: hidden;
    position: relative;
    width: 100%;
    height: 100%;
}

._numbers {
    overflow: visible;
    position: absolute;
    left: 50%;
    top: 50%;
    width: 100px;
    height: 100px;
    margin-left: -50px;
    margin-top: -50px;
}

._numbers-path {
    fill: none;
    stroke-width: 10px;
    stroke: var(--d-bg);
    stroke-linecap: round;
    stroke-linejoin: round;
    stroke-dasharray: 0, 518.055065155;
    stroke-dashoffset: 0;
    animation: numAnim 4s ease-in-out infinite;
    opacity: 0;
}

._text_ready,
._text_go {
    z-index: 3;
    opacity: 0;
    position: absolute;
    left: 50%;
    top: 0;
    width: 500px;
    height: 100%;
    margin-left: -250px;
    text-align: center;
    line-height: 140px;
    font-size: 100px;
    color: var(--d-bg);
    transform: translateX(10px);
}

._text_ready {
    animation: hideReady 4s;
}

._text_go {
    animation: hideGo 5s infinite;
}

@keyframes contAnim {

    15%,
    100% {
        margin-left: -250px;
        width: 500px;
    }

    25%,
    90% {
        margin-left: -70px;
        width: 140px;
    }
}

@keyframes numAnim {
    15% {
        stroke-dasharray: 0, 518.055065155;
        stroke-dashoffset: 0;
        opacity: 0;
    }

    25%,
    41% {
        opacity: 1;
        stroke-dasharray: 144.4256591797, 518.055065155;
        stroke-dashoffset: -40;
    }

    53%,
    66% {
        stroke-dasharray: 136.0216217041, 518.055065155;
        stroke-dashoffset: -227.238697052;
    }

    76% {
        stroke-dasharray: 113.4751205444, 518.055065155;
        stroke-dashoffset: -445.8995704651;
    }

    88%,
    100% {
        stroke-dasharray: 72.1554946899, 518.055065155;
        stroke-dashoffset: -445.8995704651;
    }

    92% {
        opacity: 1;
    }

    100% {
        opacity: 0;
    }
}

@keyframes rotation {
    to {
        transform: rotate(360deg);
    }
}

@keyframes CountdownAnim {
    15% {
        border-radius: 20px;
        transform: rotate(0);
    }

    30%,
    43% {
        border-radius: 50%;
        transform: rotate(360deg);
    }

    52%,
    65% {
        border-radius: 0;
        transform: rotate(720deg);
    }

    78%,
    90% {
        border-radius: 50%;
        transform: rotate(1080deg);
    }

    100% {
        border-radius: 20px;
        transform: rotate(1440deg);
    }
}

@keyframes hideReady {
    15% {
        opacity: 1;
    }

    20%,
    96% {
        opacity: 0;
    }
}

@keyframes hideGo {
    89% {
        opacity: 1;
    }

    0%,
    76% {
        opacity: 0;
    }
}

@keyframes fade {

    6%,
    84% {
        opacity: 1;
    }

    100% {
        opacity: 0;
    }
}
</style>
