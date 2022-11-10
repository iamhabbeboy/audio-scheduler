<script lang="ts">
</script>

<template>
<h1>Worship</h1>
<b>Selected directory: {{path}} </b>
<input type="file" @change="openDialog" />
<label>Set time</label>
<input type="datetime-local" />
<button @click="playSong">Play</button>  
<button @click="handleSchedule">Schedule </button>  
</template>

<script lang="ts">
import { ref, computed} from 'vue'
const Store = require('electron-store');

export default {
  setup() {
    const store = new Store();
    const path = computed(() => store.get('selected-directory'))

    const openDialog = (e: any) => {
      e.preventDefault()
      window.postMessage({
        type: 'select-dirs'
      })
    }

    const playSong = () => {
      let audio = new Audio('./song.mp3')
      audio.play()
    }

    const handleSchedule = () => {
      
    }

    return {
      path,
      playSong,
      openDialog,
      handleSchedule,
    }
  }
}
</script>

