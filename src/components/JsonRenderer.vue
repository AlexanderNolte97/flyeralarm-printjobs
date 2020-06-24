<template>
    <div class="uninitialized" v-if="json.hasOwnProperty('state')">
        <h1 style="text-align: center; color: red;" class="mt-4">UNINITIALIZED!</h1>
    </div>
    <div class="container" v-else>

            <h1>Pool {{ json.PoolName }}</h1>

            <div class="row">
            <ul class="list-group ml-2 mt-2" v-for="(value,key) in json.Parameters" v-bind="json.Parameters" v-bind:key="key">
                <li class="list-group-item">{{ key }}: {{ value }}</li>
            </ul>
        </div>
        <div class="row">
            <h3 class="mt-5">Timeslots:</h3>
            <table class="table mt-2">
                <tr>
                    <th>Start</th>
                    <th>Ende</th>
                    <th>Dauer</th>
                    <th>Typ</th>
                </tr>
                <tr v-for="timeSlot in json.Timeslots" v-bind="json.Timeslots" v-bind:key="timeSlot">
                    <td>{{ timeSlot.Parameters.BuildTime }}</td>
                    <td>{{ convertStartAndDurationToClock(timeSlot.Parameters.BuildTime,
                        convertPrintDurationToClock(timeSlot.Parameters.PrintDuration)) }}
                    </td>
                    <td>{{ convertPrintDurationToClock(timeSlot.Parameters.PrintDuration) }}</td>
                    <td>{{ timeSlot.Type }}</td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>

    export default {
        name: "JsonRenderer",
        data() {
            return {
                json: {"poolName": "Undefined", "Parameters": {}, "Timeslots": {}, "state": "uninitialized!"},


            }
        },
        methods: {
            convertPrintDurationToClock(duration) {
                let newStr = duration.substr(2);
                newStr = newStr.replace("H", ":");
                if (newStr.includes("M"))
                    newStr = newStr.replace("M", "");
                else
                    newStr += "00";
                return newStr;
            },
            convertStartAndDurationToClock(startClock, duration) {
                let startTime = startClock.split(":");
                let durationTime = duration.split(":");
                let startTimeInMinutes = parseInt(startTime[0]) * 60 + parseInt(startTime[1]);
                let durationTimeInMinutes = parseInt(durationTime[0]) * 60 + parseInt(durationTime[1]);
                let endTimeInMinutes = startTimeInMinutes + durationTimeInMinutes;
                let endHours = Math.floor(endTimeInMinutes / 60);
                let endMinutes = (endTimeInMinutes - endHours * 60);
                if (endMinutes < 10) {
                    return endHours + ":0" + endMinutes
                }
                return endHours + ":" + endMinutes;


            },
            fetchJson() {
                fetch('/pool.json').then(response => response.json()).then((data) => this.json = data);
            }
        },

        created() {
            this.fetchJson();
            setInterval(this.fetchJson, 3000);
        }


    }
</script>

<style scoped>

</style>