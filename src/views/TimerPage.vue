<template>
    <Timer @sendTime="recieveTime($event)" />
    <div id="times-container" style="height: 50vh">
        <ul style="height: 50vh" class="overflow-y-scroll">
            <p style="display: sticky">Times:</p>
            <li v-for="time in displayTimes">{{ time.time }}</li>
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
        type DisplayTime = { time: string; id: number };
        let storredTimes: Time[] = [];
        let displayTimes = ref<DisplayTime[]>([]);
        let currentId = 0;

        // functions
        function recieveTime(time: number) {
            currentId = currentId == 0 ? 0 : ++currentId;
            storredTimes.push({ time: Number(time.toString().slice(0, -1)), id: currentId });
            displayTimes.value.push({ time: formatTime(time.toString().slice(0, -1)), id: currentId });
            Cookies.set("currentId", currentId, { expires: 7200 /*about 20 years*/ });
            Cookies.set("storredTimes", JSON.stringify(storredTimes), { expires: 7200 });
            // sort the array
            storredTimes.sort((a, b) => {
                if (a.id > b.id) {
                    return -1;
                } else {
                    return 1;
                }
            });
        }

        if (Cookies.get("storredTimes")) {
            currentId = Cookies.get("currentId");
            storredTimes = JSON.parse(Cookies.get("storredTimes"));
            storredTimes.forEach((time) => {
                displayTimes.value.push({ time: formatTime(time.time.toString().slice(0, -1)), id: time.id });
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
