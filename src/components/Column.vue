<script lang="ts">
import Die1 from "../assets/ND1.png";
import Die2 from "../assets/ND2.png";
import Die3 from "../assets/ND3.png";
import Die4 from "../assets/ND4.png";
import Die5 from "../assets/ND5.png";
import Die6 from "../assets/ND6.png";
import Die0 from "../assets/ND0.png";

export default {
  data() {
    return {
      rows: 3,
      rowValues: [],
      totalValue: 0,
      myTurn: true,
      dieArray: [Die1, Die2, Die3, Die4, Die5, Die6],
    };
  },
  props: ["dieValue", "columnPosition", "modifiedArray"],
  watch: {
    modifiedArray: {
      handler(newValue) {
        if (newValue.columnNumber === this.columnPosition)
          this.rowValues = newValue.newArray;
        this.totalValue = this.calculateScore();
        // this.updateParentValue(this.totalValue);
        this.updateScore(this.totalValue);
      },
      deep: true,
    },
  },
  methods: {
    applyValue() {
      if (this.rowValues.length < this.rows && this.dieValue && this.myTurn) {
        this.rowValues.push(this.dieValue);
        this.totalValue = this.calculateScore();
        this.updateScore(this.totalValue);
        this.updateParentValue(this.totalValue);
        this.$emit("resetDie");
      }
    },
    updateParentValue(value) {
      this.$emit("newTotalValue", value, this.columnPosition, this.rowValues);
    },
    updateScore(value) {
      this.$emit("updateScore", value, this.columnPosition);
    },
    calculateScore() {
      let score = 0;
      const count = {};

      // create property on object for each unique score value

      this.rowValues.forEach((element) => {
        count[element] = (count[element] || 0) + 1;
      });

      // turn the properties key/value pairs into an array

      let scoreArray = Object.entries(count);

      // assign points based on modifies

      scoreArray.forEach((entry) => {
        if (entry[1] === 2) {
          let modifiedAddedScore = parseInt(entry[0]) * 4;
          score = score + modifiedAddedScore;
        } else if (entry[1] === 3) {
          let modifiedAddedScore = parseInt(entry[0]) * 9;
          score = score + modifiedAddedScore;
        } else {
          score = score + parseInt(entry[0]);
        }
      });

      return score;
    },
  },
};
</script>

<template>
  <div>
    <div class="columnScore">{{ calculateScore() }}</div>
    <div
      class="dieBlock"
      v-for="(rows, index) in rows"
      :key="index"
      @click="applyValue"
    >
      <div v-if="rowValues[index]" class="playedDie">
        <img class="dieImage" :src="dieArray[rowValues[index] - 1]" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.dieBlock {
  border: 1px solid #4e495f;
  border-radius: 3px;
  width: 40px;
  height: 40px;
  margin: 5px auto;
  display: grid;
  justify-content: center;
  align-content: center;
  background: #997577;
}

.playedDie {
  border: 1px solid black;
  width: 25px;
  height: 25px;
  display: grid;
  justify-content: center;
  align-content: center;
  margin: 0 auto;
}

.columnScore {
  text-align: center;
  margin-top: 5px;
}

.dieImage {
  width: 32px;
  height: 32px;
  image-rendering: pixelated;
}
</style>
