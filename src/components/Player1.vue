<template>
  <div class="p1">
    <div>
      <div class="score">{{ this.totalValue }}</div>
      <Die
        :p1Turn="myTurn"
        @newDieValue="(value) => handleNewDieValue(value)"
        :reset="reset"
      />
    </div>
    <div class="field">
      <Column
        :columnPosition="0"
        :modifiedArray="modifiedArray"
        :dieValue="dieValue"
        @updateScore="
          (value, columnPosition) => handleUpdateScore(value, columnPosition)
        "
        @resetDie="() => handleDieReset()"
        @newTotalValue="
          (value, columnPosition, rowValues, resetTrue) =>
            handleUpdatedValue(value, columnPosition, rowValues, resetTrue)
        "
      />
      <Column
        :dieValue="dieValue"
        :modifiedArray="modifiedArray"
        :columnPosition="1"
        @resetDie="() => handleDieReset()"
        @updateScore="
          (value, columnPosition) => handleUpdateScore(value, columnPosition)
        "
        @newTotalValue="
          (value, columnPosition, rowValues, resetTrue) =>
            handleUpdatedValue(value, columnPosition, rowValues, resetTrue)
        "
      />
      <Column
        :dieValue="dieValue"
        :modifiedArray="modifiedArray"
        :columnPosition="2"
        @updateScore="
          (value, columnPosition) => handleUpdateScore(value, columnPosition)
        "
        @resetDie="() => handleDieReset()"
        @newTotalValue="
          (value, columnPosition, rowValues, resetTrue) =>
            handleUpdatedValue(value, columnPosition, rowValues, resetTrue)
        "
      />
    </div>
    <div class="filler"></div>
  </div>
</template>

<script lang="ts">
import Die from "./Die.vue";
import Column from "./Column.vue";

export default {
  components: {
    Die,
    Column,
  },
  props: ["modifiedArray", "p1Turn"],
  data() {
    return {
      dieValue: 5,
      totalValue: 0,
      cScores: [0, 0, 0],
      reset: true,
      field: [[], [], []],
      myTurn: true,
    };
  },
  watch: {
    p1Turn(newValue) {
      this.myTurn = !this.myTurn;
    },
  },
  methods: {
    handleNewDieValue(value) {
      this.dieValue = value;
    },
    handleUpdateScore(value, columnPosition) {
      this.cScores[columnPosition] = value;
      this.totalValue = this.cScores[0] + this.cScores[1] + this.cScores[2];
    },
    handleUpdatedValue(value, columnPosition, rowValues, resetTrue) {
      this.field[columnPosition] = rowValues;
      this.$emit(
        "determineResults",
        "p1",
        columnPosition,
        rowValues,
        this.field,
        resetTrue
      );
    },
    handleDieReset() {
      this.reset = !this.reset;
    },
  },
};
</script>

<style scoped>
.field {
  display: flex;
  justify-content: space-around;
  width: 200px;
  background: #f6d6bd;
}

.p1 {
  background: #f6d6bd;
  border: 1px solid black;
  margin: 0 auto;
  width: 400px;
  display: flex;
  align-items: center;
  justify-content: space-around;
  padding: 20px;
  border-radius: 3px;
}

.score {
  text-align: center;
  border: 1px solid black;
  border-bottom: none;
}
.filler {
  width: 61px;
  background: red;
}
</style>
