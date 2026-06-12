<script setup lang="ts">
import MatrixInput from "@/components/matrix-input.vue";
import {onMounted, ref} from "vue";
let rows = ref(3);
let result = ref(0);
function spliceMatrix(matrix, column) {
  // copy matrix
  const out = matrix.map(row => row.slice());

  // remove first row
  out.shift();

  // remove cols
  for (let i = 0; i < out.length; i++) {
    out[i].splice(column, 1);
  }

  return out;
}

function determinant(matrix) {
  if (matrix.length === 1) {
    return matrix[0][0];
  }

  let result = 0;

  for (let i = 0; i < matrix.length; i++) {
    const cofactor = matrix[0][i];
    const subMatrix = spliceMatrix(matrix, i);

    if (i % 2 === 0) {
      result += cofactor * determinant(subMatrix);
    } else {
      result -= cofactor * determinant(subMatrix);
    }
  }

  return result;
}


onMounted(() => {
  document.title = "Determinant Calculator";
});

</script>

<template>
  <h1 style="text-align: center">Determinant Calculator</h1>
  <div id="container">
    <div class="label-row">
      <label for="input-n">n =</label>
      <input id="input-n" type="number" v-model.number="rows" />
    </div>
    <MatrixInput :amountRows="rows" @updateMatrix="result = determinant($event)"/>
    <h2>result = {{result}}</h2>
  </div>
</template>

<style scoped>
  #container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  #input-n {
    width: 50px;
  }
  .label-row {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 50px;
  }
</style>
