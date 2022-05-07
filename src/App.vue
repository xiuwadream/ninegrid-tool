<template>
  <div style="margin-left: 45%;">
    <div>
      <b>行列数：</b><input v-model.number="gridCount">
    </div>
    <div>
      <div>在对应格子输入数据</div>
      <div v-for="r in gridCount">
        <div class="in" v-for="rd in gridCount" style="display: inline-block;">
          <input style="width: 40px;" v-model.number="gridValues[pos(r, rd)]">
        </div>
      </div>
    </div>
    <div>
      <b>边框厚度:</b><input type="range" min="1" max="10" v-model.number="border"><span>{{ border }}</span>
    </div>
    <div>
      <b>单元格大小:</b><input type="range" min="40" max="200" v-model.number="gridSize"><span>{{ gridSize }}</span>
    </div>
    <div>
      <b>字体大小:</b><input type="range" min="16" max="96" v-model.number="fontSize"><span>{{ fontSize }}</span>
    </div>
    <table id="output">
      <tr v-for="r in gridCount">
        <td v-for="rd in gridCount">{{ gridValues[pos(r, rd)] }}</td>
      </tr>
    </table>
    <div>
      <button @click="handleDowload">导出</button>
      <a href="" id="download" download="导出表格.png" style="visibility: hidden;"></a>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import html2canvas from 'html2canvas'

const gridCount = ref(3)
const gridValues = ref(new Array(9).fill(0))

watch(gridCount, (newCount) => {
  gridValues.value = new Array(newCount * newCount).fill(0)
})

const border = ref(1)
const gridSize = ref(40)
const fontSize = ref(16)

function pos(i, j) {
  return (i - 1) * gridCount.value + (j - 1)
}
function handleDowload() {
  alert('即将开始下载图片')
  html2canvas(document.querySelector("#output")).then(function (canvas) {
    let imgUri = canvas.toDataURL("image/png").replace("image/png", "image/octet-stream");
    document.getElementById("download").setAttribute("href", imgUri);
    //手动触发a标签的下载
    document.getElementById("download").click();
  })
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
  font-size: v-bind('fontSize+"px"');
  text-align: center;
}

.in {
  width: 40px;
  margin-right: 10px;
}

button {
  width: 80px;
  height: 40px;
  margin-top: 30px;
}
</style>
