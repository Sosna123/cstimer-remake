<template>
    <div id="timer">
        <p @click="timerFunc()" class="text-center fs-1">{{ displayTime }}</p>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import formatTime from "../ts/formatTime";
export default defineComponent({
    name: "timer",
    emits: ["sendTime"],
    setup(props, ctx) {
        let start: number = 0;
        let time = ref<number>(0);
        let displayTime = ref<string>("0");
        let interval: number;
        let isTimerRunning: boolean = false;
        let storredTimes: number[] = [];

        function timerFunc() {
            if (!isTimerRunning) {
                // reset variables
                time.value = 0;
                start = Number(new Date());
                isTimerRunning = true;

                // start counting
                interval = setInterval(() => {
                    time.value = Number(new Date()) - start;
                    displayTime.value = formatTime(time.value.toString().slice(0, -1));
                }, 1);
            } else {
                // stop the timer
                isTimerRunning = false;
                clearInterval(interval);
                storredTimes.push(time.value);
                time.value = Number(new Date()) - start;
                displayTime.value = formatTime(time.value.toString().slice(0, -1));
                ctx.emit("sendTime", time.value);
            }
        }

        return {
            time,
            displayTime,
            timerFunc,
        };
    },
});
</script>

<style></style>
