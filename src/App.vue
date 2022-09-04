<template>
  <div id="app">
    <Player1
      :modifiedArray="modifiedArray1"
      :p1Turn="p1Turn"
      @determineResults="
        (playerNumber, rows, rowValues, field, resetTrue) =>
          determineResults(playerNumber, rows, rowValues, field, resetTrue)
      "
    />
    <div class="gap"></div>
    <Player2
      :p1Turn="p1Turn"
      :modifiedArray="modifiedArray2"
      @determineResults="
        (playerNumber, rows, rowValues, field, resetTrue) =>
          determineResults(playerNumber, rows, rowValues, field, resetTrue)
      "
    />
  </div>
</template>

<script>
import Player1 from "./components/Player1.vue";
import Player2 from "./components/Player2.vue";
export default {
  name: "App",
  components: { Player1, Player2 },
  data() {
    return {
      p1Field: [[], [], []],
      p2Field: [[], [], []],
      modifiedArray1: {
        columnNumber: null,
        newArray: [],
      },
      modifiedArray2: {
        columnNumber: null,
        newArray: [],
      },
      p1Turn: false,
      mostRecentPlayer: null,
    };
  },
  methods: {
    determineResults(playerNumber, rows, rowValues, field, resetTrue) {
      if (!resetTrue) {
        this.p1Turn = !this.p1Turn;
      }

      // update the state of the field
      if (playerNumber === "p1") {
        this.p1Field = field;
      } else {
        this.p2Field = field;
      }

      // now figure out if we have overlap

      if (playerNumber === "p1") {
        const newestNumber = rowValues[rowValues.length - 1];

        let p2RowToCheck = this.p2Field[rows];

        let doesP2HaveNewestNumber = p2RowToCheck.includes(newestNumber);

        // okay we have a match, now lets deal with it

        // TODO: there is a huge bug here where sometimes we fuck up the new array and insert shit that isnt there

        if (doesP2HaveNewestNumber) {
          const arrayAfterComparison = [];
          p2RowToCheck.forEach((number) => {
            if (number !== newestNumber) {
              arrayAfterComparison.push(number);
            }
          });
          this.modifiedArray2.columnNumber = rows;
          this.modifiedArray2.newArray = arrayAfterComparison;
        }
      } else {
        const newestNumber = rowValues[rowValues.length - 1];

        let p1RowToCheck = this.p1Field[rows];

        let doesP1HaveNewestNumber = p1RowToCheck.includes(newestNumber);

        // okay we have a match, now lets deal with it

        if (doesP1HaveNewestNumber) {
          const arrayAfterComparison = [];
          p1RowToCheck.forEach((number) => {
            if (number !== newestNumber) {
              arrayAfterComparison.push(number);
            }
          });
          this.modifiedArray1.columnNumber = rows;
          this.modifiedArray1.newArray = arrayAfterComparison;
        }
      }
      this.mostRecentPlayer = playerNumber;
    },
  },
};
</script>

<style>
#app {
  background-color: #08141e;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #08141e;
  height: 100vh;
  padding-top: 20px;
}
html,
body {
  padding: 0;
  margin: 0;
}
.gap {
  height: 10px;
}
</style>
