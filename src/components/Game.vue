<template>
  <div class="game">
    <div class="game-area">
      <div class="game-info">
        <p v-if="stepNumber === 0">
          Let's start! Go first with  <b :class="currentPlayer">{{ currentPlayer }}</b>!
        </p>
        <p v-else-if="!!winner">
          winner is <b :class="currentPlayer">{{ currentPlayer }}</b>!&nbsp;
          <button @click="restart">Play again</button>
        </p>
        <p v-else-if="stepNumber > 8">
          Opps! It's a draw
          <button @click="restart">Play again</button>
        </p>
        <p v-else>
          Now it's the <b :class="currentPlayer">{{ currentPlayer }}</b> player's turn
        </p>
      </div>
      <board :squares="squares" :winner="winner" @click="click" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Game',
  components: {
    Board: () => import('./Board')
  },
  data () {
    return {
      squares: Array(9).fill(null),
      stepNumber: 0,
      currentPlayer: 'X',
      winner: null
    }
  },
  methods: {
    hasWinner() {
      if (this.winner) return true

      const squares = this.squares
      const matches = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], [0, 3, 6], [1, 4, 7],
        [2, 5, 8], [0, 4, 8], [2, 4, 6]
      ]

      for (let i = 0; i < matches.length; i++) {
        const [a, b, c] = matches[i]
        if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
          this.winner = [ a, b, c ]
          return true
        }
      }

      return false
    },

    restart() {
      this.squares = Array(9).fill(null)
      this.stepNumber = 0
      this.currentPlayer = this.currentPlayer
      this.winner = null
    },

    click (i) {
      if (this.squares[i] || this.winner) return
      this.$set(this.squares, i, this.currentPlayer)
      if (!this.hasWinner()) {
        this.stepNumber++
        this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X'
      }
    }
  }
}
</script>

<style scoped>
.game {
  background-color: #2c2d46;
  background-image: var(--noise-pattern);
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.game-area {
  display: flex;
  flex-flow: column;
}


.game-info {
  margin: 0 0 3vmin;
  padding: 1rem .5rem;
  font-size: 1.25em;
  text-align: center;
  box-shadow: 2.5px 5px 25px #0001, 0 1px 6px #0004;
  background: #676786;
  color: #fff;
}

.game-info p {
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.game-info .X,
.game-info .O {
  width: 20px;
}

.game-info .X {
  color: #f34444;
}

.game-info .O {
  color: #ff9627;
}

.game-info button {
  text-transform: uppercase;
  font-weight: 600;
  font-size: .75em;
  padding: .5rem 1rem;
  margin: -.5rem 0 -.5rem 1rem;
  border: 2px solid #fff;
  background: #fff5;
  text-shadow: 0 0 1px #fff, 0 2px 5px #fff5;
  color: #111;
  cursor: pointer;
  outline: none;
  transition: all .25s ease;
}

.game-info button:focus,
.game-info button:hover {
  background: #1115;
  color: rgba(var(--theme-color));
}

.game-info button:active {
  background: #1119;
}
</style>