<script lang="ts"></script>

<template>
  <div class="container mx-auto">
    <div class="bg-purple-300 p-3">
      <h1 class="text-xlg font-bold">Audio Scheduler</h1>
    </div>
    <!-- <b>Selected directory: {{ path }} </b>
    <input type="file" @change="openDialog" />
    <label>Set time</label>
    <input type="datetime-local" />
    <button @click="playSong">Play</button>
    <button @click="handleSchedule">Schedule</button> -->
    <hr/>
    <img src="/media/play.png" width="50" height="50" @click="changeTest"/>
    <h1>Total audio in Directory ({{dir.length}})</h1>
    <div class="bg-purple-400 p-3 text-white border-b border-white cursor-pointer hover:bg-purple-500" v-for="(file, index) in dir" :key="index" @click="playSong(file)">
      {{file}}
    </div>
  </div>
</template>

<script lang="ts">
import { ref, computed } from "vue";
var cron = require("node-cron");
const Store = require("electron-store");
const fs = require("fs");

export default {
  setup() {
    const store = new Store();
    const path = computed(() => store.get("selected-directory"));
    const openDialog = (e: any) => {
      e.preventDefault();
      window.postMessage({
        type: "select-dirs",
      });
    };
    let audio = new Audio("./audio/song.mp3");
    // const audio = (file: string) => {
    //   return new Audio(file);
    // }

    const playSong = (f: string = '') => {
      // let audio = new Audio("./song.mp3");
      if(f !== '') {
        audio = new Audio(`./audio/${f}`);
      }
      if (isPlaying()) {
        console.log("currently playing");
        // return false;
      }
      audio.play();
    };

    const isPlaying = () => {
      return !audio.paused || !audio.currentTime;
    };

    const handleSchedule = () => {
      const option = {
        scheduled: false,
      };
      const job = cron.schedule("* * * * *", () => {
        console.log("running a task every minute");
        playSong();
      });
      console.log(job);
      // job.start()
    };

    const dir = computed(() => {
      return fs.readdirSync("./public/audio");
    });

    const changeTest = (e: any) => {
      if(e.target.src.includes("play.png")) {
        e.target.src = "./public/media/pause.png";
      } else {
         e.target.src = "./public/media/play.png";
      }
    }

    return {
      dir,
      path,
      playSong,
      changeTest,
      openDialog,
      handleSchedule,
    };
  },
};
</script>
<style>
/* body {
  background: #666666 !important;
} */
</style>