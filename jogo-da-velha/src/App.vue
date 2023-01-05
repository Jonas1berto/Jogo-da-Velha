<script setup>
import { ref, computed } from 'vue'

const player = ref('X')
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', '']
])

const CalculateWinner = board => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ]

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i]

    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a]
    }
  }

  return null
}

const winner = computed(() => CalculateWinner(board.value.flat()))

const MakeMove = (x, y) => {
  if (winner.value) return

  if (board.value[x][y]) return

  board.value[x][y] = player.value

  player.value = player.value === 'X' ? 'O' : 'X'
}

const ResetGame = () => {
  board.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', '']
  ]
  player.value = 'X'
}
</script>

<template>
  <main class="pt-8 text-center">
    <h1 class="mb-8 text-3xl font-bold uppercase">Jogo da Velha</h1>

    <h3 class="text-xl mb-4">Turno do Player: <span :class="` ${player === 'X' ? 'text-purple-800' : 'text-green-500'}`">{{ player }}</span>  </h3>

    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="MakeMove(x, y)"
          :class="`border border-white w-24 h-24 hover:bg-gray-700 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${
            cell === 'X' ? 'text-purple-800' : 'text-green-500'
          }`"
        >
      {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : ''}}
      </div>
      </div>
    </div>

    <h2 v-if="winner" class="text-6x1 font-bold mb-8">
      Jogador <span :class="` ${winner === 'X' ? 'text-purple-800' : 'text-green-500' } `"> {{ winner }}</span> venceu!
    </h2>

    <button @click="ResetGame" class="px-4 py-2 bg-green-900 rounded uppercase font-bold hover:bg-green-700 duration-100">Reset Game</button>
  </main>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap%27');
html {
  background: #1f2937;
  color: #fff;
  font-family: 'Press Start 2P', cursive !important;
 
}
</style>
