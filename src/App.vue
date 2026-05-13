<script setup>
import { ref, computed } from 'vue'

const tablero = ref(Array(9).fill(null))
const jugadorActual = ref('X')
const ganador = ref(null)

// Todas las combinaciones posibles para ganar
const lineasGanadoras = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8], // Horizontales
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8], // Verticales
  [0, 4, 8],
  [2, 4, 6], // Diagonales
]

const comprobarGanador = () => {
  for (let i = 0; i < lineasGanadoras.length; i++) {
    const [a, b, c] = lineasGanadoras[i]
    if (
      tablero.value[a] &&
      tablero.value[a] === tablero.value[b] &&
      tablero.value[a] === tablero.value[c]
    ) {
      ganador.value = tablero.value[a]
      return
    }
  }
}

// Si no hay ganador y todas las celdas están llenas, es empate
const esEmpate = computed(() => {
  return !ganador.value && tablero.value.every((celda) => celda !== null)
})

const realizarMovimiento = (indice) => {
  if (!tablero.value[indice] && !ganador.value) {
    tablero.value[indice] = jugadorActual.value
    comprobarGanador()

    if (!ganador.value) {
      jugadorActual.value = jugadorActual.value === 'X' ? 'O' : 'X'
    }
  }
}
</script>

<template>
  <div class="juego">
    <h1>Tres en Raya</h1>

    <div v-if="ganador" class="mensaje-fin ganador">¡Ha ganado el jugador {{ ganador }}! 🎉</div>
    <div v-else-if="esEmpate" class="mensaje-fin empate">¡Es un empate! 🤝</div>
    <p v-else>Turno del jugador: {{ jugadorActual }}</p>

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
.mensaje-fin {
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 10px;
}
.ganador {
  color: #4caf50;
}
.empate {
  color: #ff9800;
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
