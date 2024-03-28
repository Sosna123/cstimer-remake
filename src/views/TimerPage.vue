<template>
    <Timer @sendTime="recieveTime($event)" />
    <div id="times-container" style="height: 50vh">
        <ul style="height: 50vh" class="overflow-y-scroll">
            <p style="display: sticky">Times:</p>
            <li v-for="time in storredTimes">
                <div>
                    {{ time.displayTime }}
                    <button @click="addPlus2(time.id)">+2</button>
                    <button @click="deleteTime(time.id)">x</button>
                </div>
            </li>
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
        //* variables
        const Cookies = require("js-cookie");
        type Time = { time: number; displayTime: string; isPlus2: boolean; id: number };
        let storredTimes = ref<Time[]>([]);
        let currentId = -1;

        //* functions
        function recieveTime(time: number) {
            currentId++;
            //* update arrays
            storredTimes.value.push({
                time: Number(time.toString().slice(0, -1)),
                displayTime: formatTime(time.toString().slice(0, -1)),
                isPlus2: false,
                id: currentId,
            });
            //* set the cookies
            Cookies.set("storredTimes", JSON.stringify(storredTimes.value), { expires: 7200 /*about 20 years*/ });
            //* sort the arrays
            storredTimes.value.sort((a, b) => {
                if (a.id > b.id) {
                    return -1;
                } else {
                    return 1;
                }
            });
        }

        function deleteTime(id: number) {
            console.log(id);
            storredTimes.value = storredTimes.value.filter((e) => e.id != id);

            Cookies.set("storredTimes", JSON.stringify(storredTimes.value), { expires: 7200 });
        }

        function addPlus2(id: number) {
            storredTimes.value.forEach((e) => {
                if (e.id == id && !e.isPlus2) {
                    e.isPlus2 = true;
                    e.time += 200;
                    e.displayTime = formatTime(e.time.toString());
                } else if (e.id == id && e.isPlus2) {
                    e.isPlus2 = false;
                    e.time -= 200;
                    e.displayTime = formatTime(e.time.toString());
                }
            });

            Cookies.set("storredTimes", JSON.stringify(storredTimes.value), { expires: 7200 });
        }

        if (Cookies.get("storredTimes")) {
            storredTimes.value = JSON.parse(Cookies.get("storredTimes"));
            currentId = storredTimes.value[storredTimes.value.length - 1].id;
        }

        return {
            storredTimes,
            recieveTime,
            deleteTime,
            addPlus2,
        };
    },
});
</script>

<style></style>
