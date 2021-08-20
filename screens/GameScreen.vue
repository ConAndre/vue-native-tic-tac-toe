<template>
  <view class="tictactoe-board">
    <view v-for="(n, i) in 3" :key="i">
      <view v-for="(n, j) in 3" :key="j">
        <cell
          @press.native="performMove(i, j)"
          :value="board.cells[i][j]"
        ></cell>
      </view>
    </view>
    <view class="game-over-text" v-if="gameOver">
      <text>{{ gameOverText }}</text>
    </view>
  </view>
</template>

<script>
import Cell from "../components/Cell.vue";
import Board from "../classes/Board";
export default {
  components: { Cell },
  data() {
    return {
      gameOver: false,
      gameOverText: "",

      board: new Board(),
    };
  },
  methods: {
    performMove(x, y) {
      if (this.gameOver) {
        return;
      }
      if (!this.board.doMove(x, y, "x")) {
        // Invalid move.
        return;
      }
      this.$forceUpdate();
      if (this.board.isGameOver()) {
        this.gameOver = true;
        this.gameOverText = this.board.playerHas3InARow("x")
          ? "You win!"
          : "Draw";
        return;
      }
      let aiMove = this.minimax(this.board.clone(), "o");
      this.board.doMove(aiMove.move.x, aiMove.move.y, "o");
      if (this.board.isGameOver()) {
        this.gameOver = true;
        this.gameOverText = this.board.playerHas3InARow("o")
          ? "You lose!"
          : "Draw";
      }

      if (this.gameOverText == "You Win!") {
        this.winCount = this.winCount + 1;
      } else if (this.gameOverText == "You lose!") {
        this.loseCount = this.loseCount + 1;
      } else {
        this.drawCount = this.drawCount + 1;
      }

      this.$forceUpdate();
    },
    minimax(board, player, depth = 1) {
      // If the board has 3 in a row return the score of the board.
      if (board.isGameOver()) {
        return {
          score: board.getScore() + depth,
          move: null,
        };
      }
      // The 'o' player wants to maximize its score, the 'x' player wants to
      // minimize its score.
      let bestScore = player === "o" ? -10000 : 10000;
      let bestMove = null;
      let moves = board.getPossibleMoves();
      for (let i = 0; i < moves.length; i++) {
        let move = moves[i];
        let newBoard = board.clone();
        newBoard.doMove(move.x, move.y, player);
        // Recursively call the minimax function for the new board.
        const score = this.minimax(
          newBoard,
          player === "x" ? "o" : "x",
          depth + 1
        ).score;
        // If the score is better than the best saved score update the best saved
        // score to this move.
        if (
          (player === "o" && score > bestScore) ||
          (player === "x" && score < bestScore)
        ) {
          bestScore = score;
          bestMove = move;
        }
      }
      // Return the best found score & move!
      return {
        score: bestScore,
        move: bestMove,
      };
    },
  },
  watch: {
    gameOver: function (to, from) {
      if (!to) return;
      setTimeout(() => {
        this.gameOver = false;
        this.board.resetCells();
      }, 1000);
    },
  },
};
</script>

<style>
.tictactoe-board {
  display: flex;
  flex-wrap: wrap;
  width: 204px;
  height: 204px;
}
.heading {
  font-size: 30px;
  font-weight: bold;
  color: darkolivegreen;
  margin: 20px;
}
</style>
