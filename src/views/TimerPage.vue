<template>
    <Timer @sendTime="recieveTime($event)" />
    <div id="times-container">
        <ul>
            <p>Times:</p>
            <li v-for="time in displayTimes">{{ time }}</li>
        </ul>
    </div>
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
        type Time = { time: number; id: number };
        let storredTimes: Time[] = [];
        let displayTimes = ref<string[]>([]);
        let currentId = -1;

        // functions
        function recieveTime(time: number) {
            currentId++;
            storredTimes.push({ time: Number(time.toString().slice(0, -1)), id: currentId });
            displayTimes.value = [];
            Cookies.set("currentId", currentId, { expires: 7200 /*19 years*/ });
            Cookies.set("storredTimes", JSON.stringify(storredTimes), { expires: 7200 });
            // sort the array
            storredTimes.sort((a, b) => {
                if (a.id > b.id) {
                    return -1;
                } else {
                    return 1;
                }
            });
            // fill up displayTimes arr
            storredTimes.forEach((time) => {
                displayTimes.value.push(formatTime(time.time.toString()));
            });
        }

        if (Cookies.get("storredTimes")) {
            currentId = Cookies.get("currentId");
            storredTimes = JSON.parse(Cookies.get("storredTimes"));
            storredTimes.forEach((time) => {
                displayTimes.value.push(formatTime(time.time.toString()));
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
