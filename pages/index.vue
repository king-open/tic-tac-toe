<template>
  <div class="container">
    <h1>井字游戏</h1>
    <div class="board">
      <div
        class="cell"
        v-for="(cell, index) in board"
        :key="index"
        @click="makeMove(index)"
      >
        {{ cell }}
      </div>
    </div>
    <div v-if="winner">
      <p>获胜者: {{ winner }}</p>
    </div>
    <div v-else>
      <p>当前玩家: {{ currentPlayer }}</p>
    </div>
    <button @click="resetBoard">重置游戏</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: Array(9).fill(null),
      currentPlayer: "X",
      winner: null,
    };
  },
  methods: {
    makeMove(index) {
      if (!this.board[index] && !this.winner) {
        this.$set(this.board, index, this.currentPlayer);
        if (this.checkWinner()) {
          this.winner = this.currentPlayer;
        } else {
          this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
        }
      }
    },
    checkWinner() {
      const winningCombos = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];
      for (const combo of winningCombos) {
        const [a, b, c] = combo;
        if (
          this.board[a] &&
          this.board[a] === this.board[b] &&
          this.board[a] === this.board[c]
        ) {
          return true;
        }
      }
      return false;
    },
    resetBoard() {
      this.board = Array(9).fill(null);
      this.currentPlayer = "X";
      this.winner = null;
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-gap: 5px;
}

.cell {
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
  background-color: #ccc;
  cursor: pointer;
}

button {
  margin-top: 10px;
  padding: 10px 20px;
  font-size: 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>
