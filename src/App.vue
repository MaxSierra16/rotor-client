<script setup>
import { io } from "socket.io-client";
import { ref } from "vue";

const socket = io("http://localhost:3000");

const rotation = ref(360 - 18)

function moveRotor(slotNum){
  rotation.value = 360 - (((20+2) - slotNum) * 18)
}

function loadFile(){
  //@ts-ignore
  const file = document.querySelector('#commandFile').files[0]

  if(!file) return alert("No file selected")

  socket.emit('command-file', file)
}




// const socket = io("https://2ggwvnmb-3000.use.devtunnels.ms/");

// const moveHome = (e) => {
//   socket.emit('rotor_move_home');
// }

// const moveToLoad = (slot) => {
//   socket.emit('rotor_move_load', slot);
// }


socket.on("server_response", (res) => alert(res) )

</script>

<template>
  <div class="app-container">

    <h1>iSED Pro Test Client</h1>

    <!-- <div class="buttons-container">
      <h2>Click on the buttons to move to load station.</h2>

      <button v-for="n in 20" :key="n" :onclick="() => console.log(n)">slot: {{ n }}</button>
    </div> -->

    <div class="rotor" :style="{ transform: `rotate(${rotation}deg)`}">

      <div
          v-for="n in 20" class="slot" 
          :style="{ transform: `rotate(${-(n * 18)}deg) translate(100px)` }"
          :onclick="() => moveRotor(n)"
          >
          {{ n }}
        </div>
    
    </div>

    <div class="file-picker">
      <label for="commandFile">Select a file:</label>
      <input type="file" id="commandFile" name="commandFile">
      <button class="load-file" :onclick="loadFile">Load File</button>
    </div>

    <footer>
      <small>This is only for testing purposes 🧪</small>
    </footer>

  </div>
</template>

<style scoped>
.app-container {
  border: 0.5px solid;
  height: 90%;
  border-radius: 8px;

  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;

  padding: 10px;
}

.rotor {
  background: black;
  width: 200px;
  height: 200px;
  border-radius: 50%;
  
  position: relative;
  transition: transform 3s ease-in-out 0.2s;
  /* animation-direction: normal; */
}

.slot {
  background: greenyellow;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  cursor: pointer;

  position: absolute;
  top: calc(50% - 10px);
  left: calc(50% - 10px);
}

.file-picker {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

</style>
