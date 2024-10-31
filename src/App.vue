<template>
  <div v-if="!gameOver" id="board">
    <div class="square" v-for="n in 9" :key="n" @click="clickedSquare(n)">
      {{ square[n] }}
    </div>
  </div>
  <div v-if="gameOver">
    <button @click="startGame">Play a new Game</button>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  setup() {
    let currentTurn = "X";
    let square = ref([]);
    let gameOver = ref(true);

    for (let i = 0; i <= 9; i++) {
      square.value.push("empty");
      square.value[0] = "nothing";
    }
    const clickedSquare = (n) => {
      if (square.value[n] != "empty") return;

      square.value[n] = currentTurn;
      isWinner();
      isCat();
      currentTurn === "X" ? (currentTurn = "O") : (currentTurn = "X");
      console.log(currentTurn);
    };

    function isWinner() {
      const lines = [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9],
        [1, 4, 7],
        [2, 5, 8],
        [3, 6, 9],
        [1, 5, 9],
        [3, 5, 7],
      ];
      for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (
          square.value[a] !== "empty" &&
          square.value[a] === square.value[b] &&
          square.value[a] === square.value[c]
        )
          gameOver.value = true;
      }
    }

    function startGame() {
      square.value.splice(0, square.value.length);
      gameOver.value = false;
      for (let i = 0; i <= 9; i++) {
        square.value.push("empty");
        square.value[0] = "nothing";
      }
    }

    function isCat() {
      let isCat = true;
      square.value.forEach((s) => {
        if (s === "empty") isCat = false;
      });
      if (isCat === true) gameOver.value = true;
    }

    return { square, clickedSquare, gameOver, startGame };
  },
};
</script>

<style scoped>
#board {
  height: 400px;
  margin: 0 auto;
  width: 400px;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
.square {
  width: 30%;
  background-color: aquamarine;
  border: 1px solid black;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 3px;
}
</style>
