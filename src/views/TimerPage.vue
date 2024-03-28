<template>
    <Timer @sendTime="recieveTime($event)" />
    <ul id="times">
        <p>Times:</p>
        <li v-for="time in storredTimes">{{ time }}</li>
    </ul>
</template>

<script lang="ts">
import Timer from "../components/Timer.vue";
import { defineComponent, ref } from "vue";
export default defineComponent({
    components: { Timer },
    name: "timerPage",
    setup() {
        let storredTimes = ref<number[]>([]);

        function recieveTime(time: number) {
            console.log(time);
            storredTimes.value.push(time);
        }

        return {
            recieveTime,
            storredTimes,
        };
    },
});
</script>

<style></style>
