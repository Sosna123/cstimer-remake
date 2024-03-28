<template>
    <div id="times-container">
        <ul>
            <p>Times:</p>
            <li v-for="time in displayTimes">{{ time }}</li>
        </ul>
    </div>
    <Timer @sendTime="recieveTime($event)" id="timer" />
</template>

<script lang="ts">
import Timer from "../components/Timer.vue";
import formatTime from "../ts/formatTime";
import { defineComponent, ref } from "vue";
export default defineComponent({
    components: { Timer },
    name: "timerPage",
    setup() {
        // variables
        const Cookies = require("js-cookie");
        let storredTimes: number[] = [];
        let displayTimes = ref<string[]>([]);

        // functions
        function recieveTime(time: number) {
            storredTimes.push(Number(time.toString().slice(0, -1)));
            displayTimes.value = [];
            Cookies.set("storredTimes", JSON.stringify(storredTimes), { expires: 7200 /*19 years*/ });
            storredTimes.forEach((time) => {
                displayTimes.value.push(formatTime(time.toString()));
            });
        }

        if (Cookies.get("storredTimes")) {
            storredTimes = JSON.parse(Cookies.get("storredTimes"));
            storredTimes.forEach((time) => {
                displayTimes.value.push(formatTime(time.toString()));
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
