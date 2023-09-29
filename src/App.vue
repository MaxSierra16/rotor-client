<script setup>
import { io } from "socket.io-client";
import { ref } from "vue";

// const socket = io("http://localhost:3000");
const socket = io("https://2ggwvnmb-3000.use.devtunnels.ms/");

const moveHome = (e) => {
  socket.emit('rotor_move_home');
}

const moveToLoad = (slot) => {
  socket.emit('rotor_move_load', slot);
}


socket.on("server_response", ({success, slot}) => {
  if(success) alert(`Slot ${slot} move to loading position!`)
})

</script>

<template>
  <div class="app-container">

    <H1>iSED Pro Rotor Controller</H1>

    <div class="buttons-container">
      <!-- <button @click="moveHome">Move Home</button> -->
      <h2>Click on the buttons to move to load station.</h2>

      <button v-for="n in 20" :key="n" :onclick="() => moveToLoad(n)">slot: {{ n }}</button>

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

  padding: 10px;
}
</style>
