<template>
    <h1>Timer</h1>
    <button @click="timerFunc()">timer</button>
    <p>{{ time }}</p>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
export default defineComponent({
    name: "timer",
    emits: ["sendTime"],
    setup(props, ctx) {
        let start: number = 0;
        let time = ref<number>(0);
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
                }, 1);
            } else {
                // stop the timer
                isTimerRunning = false;
                clearInterval(interval);
                time.value = Number(new Date()) - start;
                storredTimes.push(time.value);
                ctx.emit("sendTime", time.value);
            }
        }

        return {
            time,
            timerFunc,
        };
    },
});
</script>

<style></style>
