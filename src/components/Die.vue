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
      value: null,
      goodToReroll: true,
      image: Die0,
      dieArray: [Die1, Die2, Die3, Die4, Die5, Die6],
    };
  },
  props: {
    reset: {
      default: false,
    },
    p1Turn: {},
  },
  watch: {
    reset(oldValue, newValue) {
      this.value = null;
      this.goodToReroll = true;
      this.$emit("newDieValue", null);
    },
    value(newValue) {
      if (newValue === null) {
        this.image = Die0;
      } else {
        this.image = this.dieArray[newValue - 1];
      }
    },
  },
  methods: {
    rollDie() {
      const newDieValue = Math.floor(Math.random() * 6) + 1;
      this.$emit("newDieValue", newDieValue);
      this.value = newDieValue;
      this.goodToReroll = false;
    },
  },
  created() {
    this.$emit("newDieValue", this.value, this.columnPosition);
  },
};
</script>

<template>
  <div class="dieContainer">
    <img class="dieImage" :src="image" />
    <button @click="rollDie" v-if="goodToReroll && !p1Turn">Roll</button>
  </div>
</template>

<style scoped>
.die {
  border: 1px solid black;
  width: 25px;
  height: 25px;
  display: grid;
  justify-content: center;
  align-content: center;
  margin: 10px auto;
}
.dieContainer {
  border: 1px solid black;
  height: 80px;
  width: 50px;
  display: grid;
  justify-content: center;
  align-content: center;
  padding: 5px;
}
.dieImage {
  width: 32px;
  height: 32px;
  image-rendering: pixelated;
  display: block;
  margin: 2px auto;
  border: 1px solid #0f2a3f;
  border-radius: 10px;
  box-sizing: initial;
}

button {
  background: #0f2a3f;
  border: 1px solid #997577;
  border-radius: 2px;
  color: #f6d6bd;
}

button:hover {
  background: #20394f;
}
</style>
