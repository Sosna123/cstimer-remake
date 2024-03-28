<template>
    <Timer @sendTime="recieveTime($event)" />
    <ul id="times">
        <p>Times:</p>
        <li v-for="time in displayTimes">{{ time }}</li>
    </ul>
</template>

<script lang="ts">
import Timer from "../components/Timer.vue";
import formatTime from "../ts/formatTime";
import { defineComponent, ref } from "vue";
export default defineComponent({
    components: { Timer },
    name: "timerPage",
    setup() {
        let storredTimes: number[] = [];
        let displayTimes = ref<string[]>([]);

        function recieveTime(time: number) {
            storredTimes.push(time);
            displayTimes.value = [];
            storredTimes.forEach((time) => {
                displayTimes.value.push(formatTime(time.toString().slice(0, -1)));
            });
        }

        return {
            recieveTime,
            displayTimes,
        };
    },
});
</script>

<style></style>
