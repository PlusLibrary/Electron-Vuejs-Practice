<template>
    <div id="main">
        <div class="time">{{ formatTime(time) }}</div>
        <div class="frame">
            <Alarm v-if="view == 'Alarm'" />
            <Stopwatch v-if="view == 'Stopwatch'" />
            <Setting v-if="view == 'Setting'" />
            <Footer v-model="view"/>
        </div>
        <MessageBox title="Hello" content="World"/>
    </div>
</template>

<script>
import Footer from "@/components/Footer.vue";
import Alarm from "@/views/Alarm.vue";
import Stopwatch from "@/views/Stopwatch.vue";
import Setting from "@/views/Setting.vue";
import MessageBox from "@/components/MessageBox.vue";

export default {
    name: "Main",
    components: { Footer, Alarm, Stopwatch, Setting, MessageBox },
    data() {
        return {
            time: {},
            view: "Alarm",
        };
    },
    computed: {
        timezoneOffset() {
            return -Math.floor(new Date().getTimezoneOffset() / 60);
        }
    },
    mounted() {
        clearInterval(localStorage.getItem("timer"));
        localStorage.setItem("timer", setInterval(() => this.time = this.currentTime(), 100));
    },
    methods: {
        currentTime() {
            let totalSecond = Math.floor(Date.now() / 1000 % 86400);
            let second = totalSecond % 60;
            let minute = Math.floor(totalSecond / 60) % 60;
            let hour = Math.floor((totalSecond / 3600 + this.timezoneOffset) % 24);
            return {"hour" : hour, "minute" : minute, "second" : second};
        },
        formatTime(time) {
            return [ time.hour, time.minute, time.second ]
                .map(v => v ?? 0)
                .map(v => (v + "").padStart(2, "0"))
                .join(":")
        },
        randomString() {
            return [...Array(32)].map(() => (~~(Math.random() * 36)).toString(36)).join("");
        },
    },
};
</script>

<style scoped>
.frame {
    position: relative;
    height: 100%;
}
.time {
    z-index: 10;
    font-family: "medium";
    background-color: #131313;
    font-size: 16px;
    text-align: center;
    padding-top: 4px;
    -webkit-app-region: drag;
}
</style> 