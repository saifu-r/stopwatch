<template>
  <div class="app">
    <div class="container">
      <div class="circle-container">
        <h1>{{ formattedTime(timeElapsed) }}</h1>
      </div>
      
      
      <button v-if="state==='stopped'" @click="startStopwatch">Start</button>
      <button v-if="state==='running'" @click="stopStopwatch">Stop</button>
      <button v-if="state==='paused'" @click="startStopwatch">Resume</button>
      <button v-if="state==='running' || state==='paused'" @click="resetStopwatch">Reset</button>
    </div>
  </div>

</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue';

export default defineComponent({
  name: 'App',
  components: {},
  setup(){
    onMounted(() => {
      updateTime();
      setInterval(updateLiveTime, 1000)
    });

    const currentTime= ref("")
    const currentDate= ref("")

    const liveTime= ref("")

    const updateTime= ()=>{
      const now= new Date()
      currentTime.value= now.toLocaleTimeString()
      currentDate.value= now.toLocaleDateString()

      
    }

    const updateLiveTime= ()=>{
      const now= new Date()
      liveTime.value= now.toLocaleTimeString()
    }

    //-----------------------------------------------------------------------------------//

    

    const timeElapsed= ref(0)
    const interval= ref<number|undefined>(undefined)
    const state= ref<'stopped'| 'running'| 'paused'>('stopped')

    const runStopwatch=() =>{
     timeElapsed.value++
    }
    const startStopwatch=() =>{  
      interval.value= setInterval(runStopwatch,1000)
      state.value= 'running'
    }

    const stopStopwatch= ()=>{
      clearInterval(interval.value)
      state.value= 'paused'
    }

    const resetStopwatch= ()=>{
      clearInterval(interval.value)
      timeElapsed.value= 0
      state.value= 'stopped'
    }

    const formattedTime= (elapsed: number)=>{
      const min= Math.floor(elapsed/ 60)
      const sec= elapsed% 60

      return `${String(min).padStart(2,'0')} : ${String(sec).padStart(2,'0')}`
    }


    return {currentTime, currentDate, updateTime, liveTime, updateLiveTime,
     timeElapsed, startStopwatch, stopStopwatch, resetStopwatch, state, formattedTime}
  }
});
</script>

<style>

body{
  width: 100%;
  height: 100%;
  /* Add your background pattern here */
  background-color: #ffffff;
  background-image: radial-gradient(rgba(12, 12, 12, 0.171) 2px, transparent 0);
  background-size: 30px 30px;
  background-position: -5px -5px;
}
.container {
  text-align: center;
  position: absolute;
  align-items: center;
  justify-content: center;
  padding: 20px;
  margin: 20px;
  top: 30%;
  left: 50%;
  transform: translate(-50%, -50%);
  /* border: 3px solid black; */
  border-radius:20px;
  /* Your container styles */
  width: 40%;
  height: 50%;
  background-color: #D9D9D9;
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.379);
  font-family: Helvetica;
}
button {
  margin: 20px;
  width: fit-content;
  min-width: 100px;
  height: 45px;
  padding: 8px;
  border-radius: 5px;
  border: 2.5px solid #E0E1E4;
  box-shadow: 0px 0px 20px -20px;
  cursor: pointer;
  background-color: white;
  transition: all 0.2s ease-in-out 0ms;
  user-select: none;
  font-family: 'Poppins', sans-serif;
  border: 8px;
}

button:hover {
  background-color: #F2F2F2;
  box-shadow: 0px 0px 20px -18px;
}

button:active {
  transform: scale(0.95);
}

.circle-container {
  margin: 0 auto;
  position: relative;
  width: 200px;
  height: 200px;
  border-radius: 50%;
  border: 4px solid #717171;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: #a8a8a8;
  
}

</style>
