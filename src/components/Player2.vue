<template>
  <div class="p1">
    <div class="filler"></div>
    <div class="field">
      <Column
        :columnPosition="0"
        :modifiedArray="modifiedArray"
        :dieValue="dieValue"
        @resetDie="() => handleDieReset()"
        @newTotalValue="
          (value, columnPosition, rowValues) =>
            handleUpdatedValue(value, columnPosition, rowValues)
        "
      />
      <Column
        :dieValue="dieValue"
        :modifiedArray="modifiedArray"
        :columnPosition="1"
        @resetDie="() => handleDieReset()"
        @newTotalValue="
          (value, columnPosition, rowValues) =>
            handleUpdatedValue(value, columnPosition, rowValues)
        "
      />
      <Column
        :dieValue="dieValue"
        :modifiedArray="modifiedArray"
        :columnPosition="2"
        @resetDie="() => handleDieReset()"
        @newTotalValue="
          (value, columnPosition, rowValues) =>
            handleUpdatedValue(value, columnPosition, rowValues)
        "
      />
    </div>
    <div>
      <div class="score">{{ this.totalValue }}</div>
      <Die @newDieValue="(value) => handleNewDieValue(value)" :reset="reset" />
    </div>
  </div>
</template>

<script lang="ts">
import Die from "./Die.vue";
import Column from "./Column.vue";

export default {
  props: ["modifiedArray"],
  components: {
    Die,
    Column,
  },
  data() {
    return {
      dieValue: 5,
      totalValue: 0,
      cScores: [0, 0, 0],
      reset: true,
      field: [[], [], []],
    };
  },

  methods: {
    handleNewDieValue(value) {
      this.dieValue = value;
    },
    handleUpdatedValue(value, columnPosition, rowValues) {
      this.cScores[columnPosition] = value;
      this.totalValue = this.cScores[0] + this.cScores[1] + this.cScores[2];
      this.field[columnPosition] = rowValues;

      this.$emit(
        "determineResults",
        "p2",
        columnPosition,
        rowValues,
        this.field
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
}

.score {
  text-align: center;
  border: 1px solid black;
  border-bottom: none;
}

.filler {
  width: 59px;
  background: red;
}
</style>
