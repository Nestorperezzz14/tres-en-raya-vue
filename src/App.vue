<template>
  <div class="juego">
    <h1>Tres en Raya</h1>
    <div class="tablero">
      <div class="celda" v-for="n in 9" :key="n"></div>
    </div>
  </div>
</template>

<style>
.juego {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Arial, sans-serif;
}
.tablero {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  gap: 5px;
  background-color: black;
  padding: 5px;
}
.celda {
  width: 100px;
  height: 100px;
  background-color: white;
}
</style>
s
