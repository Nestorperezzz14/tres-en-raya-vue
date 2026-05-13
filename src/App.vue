<script setup>
import { ref } from 'vue'

// El estado del tablero (9 casillas vacías al empezar)
const tablero = ref(Array(9).fill(null))
// El jugador que empieza
const jugadorActual = ref('X')

// Función que se ejecuta al hacer clic en una celda
const realizarMovimiento = (indice) => {
  // Solo ponemos marca si la celda está vacía
  if (!tablero.value[indice]) {
    tablero.value[indice] = jugadorActual.value
    // Cambiamos de jugador (Si era X, ahora O; y viceversa)
    jugadorActual.value = jugadorActual.value === 'X' ? 'O' : 'X'
  }
}
</script>

<template>
  <div class="juego">
    <h1>Tres en Raya</h1>
    <p>Turno del jugador: {{ jugadorActual }}</p>

    <div class="tablero">
      <div v-for="(celda, i) in tablero" :key="i" class="celda" @click="realizarMovimiento(i)">
        {{ celda }}
      </div>
    </div>
  </div>
</template>

<style>
.juego {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Arial, sans-serif;
  margin-top: 50px;
}
.tablero {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  gap: 5px;
  background-color: #333;
  padding: 5px;
  border-radius: 8px;
}
.celda {
  width: 100px;
  height: 100px;
  background-color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
  font-weight: bold;
  cursor: pointer;
}
.celda:hover {
  background-color: #f0f0f0;
}
</style>
