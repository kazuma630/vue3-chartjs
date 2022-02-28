<template>
  <DoughnutChart v-bind="doughnutChartProps" />
</template>

<script lang='ts'>
  import { computed, ref } from "vue";
  import { DoughnutChart, useDoughnutChart } from "vue-chart-3";
  import { Chart, ChartData, ChartOptions, registerables } from "chart.js";
  
  Chart.register(...registerables);

  export default {
    name: "App",
    components:{ DoughnutChart },
    setup() {
      const dataValues = ref([70, 50, 20, 10, 5]);
      const toggleLegend = ref(true);

      const testData = computed<ChartData<"doughnut">>(() => ({
        labels: ["ruby", "javascript", "css", "aws", "php"],
        datasets: [
          {
            data: dataValues.value,
            backgroundColor: [
              "#ff0000",
              "#ffff00",
              "#b0c4de",
              "#daa520",
              "#6a5acd",
            ],
          },
        ],
      }));

      const options = computed<ChartOptions<"doughnut">>(() => ({
        scales: {
          myScale: {
            type: "logarithmic",
            position: toggleLegend.value ? "left" : "right",
          },
        },
        plugins: {
          legend: {
            position: toggleLegend.value ? "top" : "bottom",
          },
          title: {
            display: true,
            text: "使用言語の割合",
          },
        },
      }));

      const { doughnutChartProps, doughnutChartRef } = useDoughnutChart({
        chartData: testData,
        options,
      });

      // function shuffleData() {
      //   dataValues.value = shuffle(dataValues.value);
      //   console.log(doughnutChartRef.value.chartInstance);
      // }

      // function switchLegend() {
      //   toggleLegend.value = !toggleLegend.value;
      // }

      return {
        testData,
        options,
        doughnutChartRef,
        doughnutChartProps,
      };
    }
  };
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
