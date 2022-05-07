<template>
  <div style="margin-left: 45%;">
    <div>
      <b>行列数：</b><input v-model.number="gridCount">
    </div>
    <div>
      <div>在对应格子输入数据</div>
      <div v-for="r in gridCount">
        <div class="in" v-for="rd in gridCount" style="display: inline-block;">
          <input style="width: 40px;"  v-model.number="gridValues[pos(r, rd)]">
        </div>
      </div>
    </div>
    <div>
      <b>边框厚度:</b><input type="range" min="1" max="10" v-model.number="border"><span>{{border}}</span>
    </div>
    <div>
      <b>单元格大小:</b><input type="range" min="40" max="200" v-model.number="gridSize"><span>{{gridSize}}</span>
    </div>
    <table>
      <tr v-for="r in gridCount">
        <td v-for="rd in gridCount">{{ gridValues[pos(r, rd)] }}</td>
      </tr>
    </table>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
const gridCount = ref(3)
const gridValues = ref(new Array(9).fill(0))

watch(gridCount,(newCount) => {
  gridValues.value = new Array(newCount*newCount).fill(0)
})

const border = ref(1)
const gridSize = ref(40)

function pos(i, j) {
  return (i - 1) * gridCount.value + (j - 1)
}
</script>

<style>
table {
  margin-top: 40px;
  border-collapse: collapse;
  border-spacing: 0;
}

table td {
  border: solid black;
  border-width: v-bind('border+"px"');
  margin: 0;
  width: v-bind('gridSize+"px"');
  height: v-bind('gridSize+"px"');
  text-align: center;
}
.in {
 width: 40px;
 margin-right: 10px;
}
</style>
