<template>
  <div
    id="home"
    class="container"
  >
    <h1>Tic Tac Toe</h1>
    <button
      @click="startGame()"
      type="button"
      class="btn btn-success"
    >Start Game 🎮</button>
    <br /><br />

    <p class="h2">{{ activePlayer  }} - {{ players[activePlayerIndex] }} </p>
    <div
      v-for="(row, index) in board"
      :key="row.id"
      class="row"
    >
      <div
        v-for="col in board[index]"
        :key="col.id"
        :id="col"
        @click="move"
        class="col"
      >
        <div v-if='col === "O" || col === "X"'>{{ col }} </div>
      </div>
    </div>

  </div>

</template> 


    <script>
import Vue from "vue";
export default {
  name: "Home",
  data() {
    return {
      board: [],
      players: ["O", "X"],
      activePlayerIndex: 0,
      gameOver: true,
      moves: 0,
    };
  },
  computed: {
    activePlayer: function () {
      if (this.gameOver) {
        return "Click Start Button";
      }
      if (this.activePlayerIndex === 0) {
        return "Player 1";
      } else {
        return "Player 2";
      }
    }

  },
  props: {},
  methods: {
    move(event) {
      if (!this.gameOver) {
        if (!this.players.includes(event.target.textContent.trim())) {
          this.moves++;
          Vue.set(this.board[event.target.id[0]], event.target.id[1], this.players[this.activePlayerIndex]);
          this.gameOverCheck();
          this.activePlayerIndex === 0 ? this.activePlayerIndex = 1 : this.activePlayerIndex = 0;
        }
      } else {
        this.makeToast('Game has not been started', "Please start a game");
      }
    },
    gameOverCheck() {
      const playerSymbol = this.players[this.activePlayerIndex];
      // na skos
      if ((this.board[0][0] === playerSymbol && this.board[1][1] === playerSymbol && this.board[2][2] === playerSymbol) || (this.board[2][0] === playerSymbol && this.board[1][1] === playerSymbol && this.board[0][2] === playerSymbol)) {
        this.gameWon();
      }
      for (let i = 0; i < 3; i++) {
        // w poziomie
        if (this.board[i][0] === playerSymbol && this.board[i][1] === playerSymbol && this.board[i][2] === playerSymbol) {
          this.gameWon();
        }
        // w pionie
        if (this.board[0][i] === playerSymbol && this.board[1][i] === playerSymbol && this.board[2][i] === playerSymbol) {
          this.gameWon();
        }
      }
      //remis
      if (this.moves > 8 && this.gameOver === false) {
        this.draw();
      }
    },
    gameWon() {
      this.makeToast(`${this.activePlayer} wins`, "Congratulation");
      this.gameOver = true;
    },
    draw() {
      this.makeToast("It's a draw!", "Try again");
      this.gameOver = true;
    },
    makeToast(title, text) {
      this.toastCount++
      this.$bvToast.toast(text, {
        title: title,
        autoHideDelay: 5000,
        appendToast: false
      })
    },
    generateBoardArray() {
      this.board = [["00", "01", "02"], ["10", "11", "12"], ["20", "21", "22"]]
    },
    startGame() {
      this.gameOver = false;
      this.activePlayerIndex = 0;
      this.generateBoardArray();
      this.moves = 0;
    },
    fieldSelected() {

    }
  },
  beforeMount() {
    //this.generateBoardArray();
  }
};
    </script>

    <style scoped>
.col {
  padding: 50px;
  cursor: pointer;
  border: 2px solid gray;
}
</style>
