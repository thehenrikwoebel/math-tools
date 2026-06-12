<script setup lang="ts">
import { ref, watch } from "vue";

const props = defineProps({
  amountRows: Number,
});

const emit = defineEmits(["updateMatrix"]);

const matrix = ref<number[][]>([]);

watch(
    () => props.amountRows,
    (newSize) => {
      matrix.value = Array.from({ length: newSize }, () =>
          Array.from({ length: newSize }, () => 0)
      );
    },
    { immediate: true }
);

function sendMatrix() {
  emit("updateMatrix", matrix.value);
}
</script>

<template>
  <div>
    <div class="row" v-for="(row, i) in matrix" :key="i">
      <input
          v-for="(column, j) in row"
          :key="`${i}-${j}`"
          type="number"
          class="input-field"
          v-model.number="matrix[i][j]"
      />
    </div>

    <div id="result-container">
      <button id="button-calc" @click="sendMatrix">
        calculate
      </button>
    </div>
  </div>
</template>

<style scoped>
.input-field {
  width: 50px;
}
#result-container {
  display: flex;
  justify-content: center;
}
#button-calc {
  margin-top: 50px;
}
</style>