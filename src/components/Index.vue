<template>
  <div class="flex">
    <div class="mb-6 mx-4">
      <label class="mr-4 text-gray-700 font-bold inline-block mb-2">Fecha de Inicio:</label>
      <input type="date"  class="border bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded" v-model="startDate" />
    </div>
    <div class="mb-6 mx-4">
        <label class="mr-4 text-gray-700 font-bold inline-block mb-2">Fecha de Cierre:</label>
        <input type="date"  class="border bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded" v-model="finishDate"/>
    </div>
    <button class="mb-6 px-2 bg-blue-400 text-white font-bold rounded" @click="getData">Búscar</button>
  </div>
  <v-chart class="chart" :option="option" />
</template>

<script>
import { use } from "echarts/core";
import { CanvasRenderer } from "echarts/renderers";
import { BarChart } from "echarts/charts";
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent
} from "echarts/components";
import VChart, { THEME_KEY } from "vue-echarts";
import { ref, defineComponent, onMounted } from "vue";
import axios from 'axios'

use([
  CanvasRenderer,
  BarChart,
  TitleComponent,
  TooltipComponent,
  LegendComponent
]);

export default defineComponent({
  name: "Index",
  components: {
    VChart
  },
  provide: {
    [THEME_KEY]: "dark"
  },
  setup () {
    const startDate = ref('2021-09-01')
    const finishDate = ref('2021-09-30')
    let dataLabels = ref([])
    let dataValues = ref([])
    const option = ref({});
    onMounted(() => {
      getData();
    })    
    const getData = ()=>{
      dataLabels.value = []
      dataValues.value = []
      axios.get(`${process.env.VUE_APP_HOST}/v1/invoice/${startDate.value}/${finishDate.value}`)
        .then(response =>{
          dataLabels.value = response.data.map(item => {
            dataLabels.value.push(item.name)
          });
          dataValues.value = response.data.map(item => {
            dataValues.value.push(item.value)
          });
        })
        .catch((err) =>{
          console.log(err)
        });
      option.value = {
        xAxis: {
          type: 'category',
          data: dataLabels.value
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            data: dataValues.value,
            type: 'bar',
            showBackground: true,
            backgroundStyle: {
              color: 'rgba(180, 180, 180, 0.2)'
            }
          }
        ]
      }
    } 
    return { 
      option,
      startDate,
      finishDate,
      getData,
    };
  }
});
</script>

<style scoped>
.chart {
  height: 400px;
}
</style>
