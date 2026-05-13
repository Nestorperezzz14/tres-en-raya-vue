<script setup>
import { ref, computed } from 'vue'

const tablero = ref(Array(9).fill(null))
const jugadorActual = ref('X')
const ganador = ref(null)

// Marcadores de puntuación
const victoriasX = ref(0)
const victoriasO = ref(0)

const lineasGanadoras = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
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
      // Sumamos puntuación al ganador
      if (ganador.value === 'X') victoriasX.value++
      else victoriasO.value++
      return
    }
  }
}

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

const reiniciarJuego = () => {
  tablero.value = Array(9).fill(null)
  jugadorActual.value = 'X'
  ganador.value = null
}
</script>

<template>
  <div class="juego">
    <h1>Tres en Raya</h1>

    <div class="marcador">
      <div class="puntos">
        Jugador X: <span>{{ victoriasX }}</span>
      </div>
      <div class="puntos">
        Jugador O: <span>{{ victoriasO }}</span>
      </div>
    </div>

    <div v-if="ganador" class="mensaje-fin ganador">¡Ganador: {{ ganador }}! 🏆</div>
    <div v-else-if="esEmpate" class="mensaje-fin empate">¡Empate! 🤝</div>
    <p v-else>
      Turno de: <strong>{{ jugadorActual }}</strong>
    </p>

    <div class="tablero">
      <div v-for="(celda, i) in tablero" :key="i" class="celda" @click="realizarMovimiento(i)">
        {{ celda }}
      </div>
    </div>

    <button class="btn-reinicio" @click="reiniciarJuego">Nueva Partida</button>
  </div>
</template>

<style>
.juego {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: 'Segoe UI', Arial;
  margin-top: 30px;
}
.marcador {
  display: flex;
  gap: 30px;
  margin-bottom: 20px;
  background: #eee;
  padding: 10px 20px;
  border-radius: 10px;
}
.puntos {
  font-size: 1.2rem;
  font-weight: bold;
}
.puntos span {
  color: #2196f3;
  font-size: 1.5rem;
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
  font-size: 2.5rem;
  font-weight: bold;
  cursor: pointer;
  transition: 0.2s;
}
.celda:hover {
  background-color: #f9f9f9;
  transform: scale(1.02);
}
.btn-reinicio {
  margin-top: 20px;
  padding: 12px 25px;
  font-size: 1rem;
  background-color: #2196f3;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
}
.btn-reinicio:hover {
  background-color: #1976d2;
}
</style>
