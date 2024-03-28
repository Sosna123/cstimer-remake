<template>
    <h1>Timer</h1>
    <button @click="timerFunc()">timer</button>
    <p>{{ time }}</p>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
export default defineComponent({
    name: "timer",
    setup() {
        let start: number = 0;
        let time = ref<Number>(0);
        let interval: number;
        let isTimerRunning: boolean = false;

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
