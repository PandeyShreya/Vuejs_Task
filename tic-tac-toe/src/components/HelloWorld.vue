<template>
  <div class="tic-tac-toe">
    <h1>Tic-Tac-Toe</h1>
    <div
      v-for="(row, rowIndex) in board"
      :key="'row-' + rowIndex"
      class="row"
    >
      <button
        v-for="(cell, cellIndex) in row"
        :key="'cell-' + cellIndex"
        @click="makeMove(rowIndex, cellIndex)"
        :disabled="cell !== null"
        :class="{
          cell: true,
          'winning-cell': isWinningCell(rowIndex, cellIndex)
        }"
      >
        {{ cell }}
      </button>
    </div>
    <p v-if="winner" class="result">{{ winner }} wins!</p>
    <p
      v-if="!winner && board.flat().every(cell => cell !== null)"
      class="result"
    >
      It's a draw!
    </p>
    <button
      v-if="winner || board.flat().every(cell => cell !== null)"
      @click="resetGame"
      class="reset-button"
    >
      Reset Game
    </button>
  </div>
</template>

<script>
export default {
  name: 'TicTacToe',
  data() {
    return {
      board: [
        [null, null, null],
        [null, null, null],
        [null, null, null]
      ],
      currentPlayer: 'X',
      winner: null,
      winningLine: null // New data property to track the winning line
    };
  },
  methods: {
    makeMove(row, col) {
      if (!this.board[row][col] && !this.winner) {
        this.$set(this.board[row], col, this.currentPlayer);
        this.checkWinner();
        this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
      }
    },
    checkWinner() {
      const lines = [
        // Rows
        [[0, 0], [0, 1], [0, 2]],
        [[1, 0], [1, 1], [1, 2]],
        [[2, 0], [2, 1], [2, 2]],
        // Columns
        [[0, 0], [1, 0], [2, 0]],
        [[0, 1], [1, 1], [2, 1]],
        [[0, 2], [1, 2], [2, 2]],
        // Diagonals
        [[0, 0], [1, 1], [2, 2]],
        [[0, 2], [1, 1], [2, 0]]
      ];

      for (const line of lines) {
        const [a, b, c] = line;
        if (this.board[a[0]][a[1]] &&
            this.board[a[0]][a[1]] === this.board[b[0]][b[1]] &&
            this.board[a[0]][a[1]] === this.board[c[0]][c[1]]) {
          this.winner = this.board[a[0]][a[1]];
          this.winningLine = line;
          return;
        }
      }
      this.winningLine = null; // No winner
    },
    resetGame() {
      this.board = [
        [null, null, null],
        [null, null, null],
        [null, null, null]
      ];
      this.currentPlayer = 'X';
      this.winner = null;
      this.winningLine = null; // Reset winning line
    },
    isWinningCell(row, col) {
      return this.winningLine && this.winningLine.some(([r, c]) => r === row && c === col);
    }
  }
};
</script>

<style scoped>
.tic-tac-toe {
  text-align: center;
  font-family: Arial, sans-serif;
  margin: 20px auto;
  width: fit-content;
  border: 2px solid #333;
  border-radius: 8px;
  padding: 20px;
  background-color: #f9f9f9;
  position: relative;
}

h1 {
  font-size: 2.5em;
  margin-bottom: 20px;
}

.row {
  display: flex;
  justify-content: center;
  margin: 10px 0;
  position: relative; /* Ensure positioning context for pseudo-elements */
}

.cell {
  font-size: 2em;
  width: 60px;
  height: 60px;
  margin: 5px;
  padding: 0;
  box-sizing: border-box;
  cursor: pointer;
  background-color: #fff;
  border: 2px solid #ddd;
  border-radius: 5px;
  transition: background-color 0.3s, transform 0.2s;
  position: relative; /* Ensure positioning context for pseudo-elements */
}

.cell:hover:not(:disabled) {
  background-color: #e0e0e0;
  transform: scale(1.05);
}

.cell:disabled {
  background-color: #f0f0f0;
  cursor: not-allowed;
}

.winning-cell {
  background-color: #dff0d8; /* Light green background for winning cells */
  color: #3c763d; /* Dark green text color */
}

.result {
  font-size: 1.5em;
  margin: 20px 0;
  color: #333;
}

.reset-button {
  font-size: 1em;
  padding: 10px 20px;
  margin: 20px;
  cursor: pointer;
  border: 2px solid #007bff;
  border-radius: 5px;
  background-color: #007bff;
  color: #fff;
  transition: background-color 0.3s, transform 0.2s;
}

.reset-button:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

.reset-button:active {
  background-color: #004494;
}

/* Winning line styles */
.tic-tac-toe::before,
.tic-tac-toe::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 2px;
  background-color: #007bff;
  transition: transform 0.3s;
}

.tic-tac-toe::before {
  top: 0;
  left: 0;
  transform: translateY(-50%) scaleX(0);
}

.tic-tac-toe::after {
  left: 0;
  right: 0;
  bottom: 0;
  transform: translateY(50%) scaleX(0);
}

.winning-row::before {
  transform: translateY(-50%) scaleX(1);
}

.winning-column::after {
  transform: translateY(50%) scaleX(1);
}

.winning-diagonal {
  position: relative;
}

.winning-diagonal::before {
  content: '';
  position: absolute;
  width: 140%;
  height: 2px;
  background-color: #007bff;
  top: 50%;
  left: -20%;
  transform: rotate(45deg);
}

.winning-diagonal::after {
  content: '';
  position: absolute;
  width: 140%;
  height: 2px;
  background-color: #007bff;
  bottom: 50%;
  left: -20%;
  transform: rotate(-45deg);
}
</style>
