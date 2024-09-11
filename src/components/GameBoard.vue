<template>
  <div class="board">
    <div v-for="(cell, index) in board" :key="index" class="cell" @click="makeMove(index)">
      {{ cell }}
    </div>
    <div v-if="winner" class="winner">
      {{ winner }} wins!
    </div>
    <div v-else-if="isDraw" class="winner">
      Draw!
    </div>
    <button @click="resetGame" class="reset-button">Reset Game</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const board = ref(Array(9).fill(null));
const currentPlayer = ref('X');
const winner = ref(null);
const isDraw = ref(false);

const winningCombinations = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6]
];

const makeMove = (index) => {
  if (!board.value[index] && !winner.value) {
    board.value[index] = currentPlayer.value;
    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
    checkWinner();
  }
};

const checkWinner = () => {
  for (const combination of winningCombinations) {
    const [a, b, c] = combination;
    if (board.value[a] && board.value[a] === board.value[b] && board.value[a] === board.value[c]) {
      winner.value = board.value[a];
      return;
    }
  }
  if (!board.value.includes(null)) {
    isDraw.value = true;
  }
};

const resetGame = () => {
  board.value = Array(9).fill(null);
  currentPlayer.value = 'X';
  winner.value = null;
  isDraw.value = false;
};
</script>

<style scoped>
.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 10px;
  justify-content: center;
  align-items: center;
  margin: 20px auto;
  max-width: 320px;
}

.cell {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 100px;
  background-color: #f0f0f0;
  border: 2px solid #ccc;
  font-size: 2rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

.cell:hover {
  background-color: #e0e0e0;
}

.winner {
  grid-column: span 3;
  text-align: center;
  font-size: 1.5rem;
  margin-top: 20px;
}

.reset-button {
  grid-column: span 3;
  padding: 10px 20px;
  font-size: 1rem;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  transition: background-color 0.3s;
}

.reset-button:hover {
  background-color: #0056b3;
}
</style>