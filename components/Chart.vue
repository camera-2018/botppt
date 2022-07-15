<template>
  <v-chart class="chart" :option="option" />
</template>

<script>
import { use } from "echarts/core";
import { SVGRenderer } from "echarts/renderers";
import { PieChart } from "echarts/charts";
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent
} from "echarts/components";
import VChart, { THEME_KEY } from "vue-echarts";
import { ref, defineComponent } from "vue";

use([
  SVGRenderer,
  PieChart,
  TitleComponent,
  TooltipComponent,
  LegendComponent
]);

export default defineComponent({
  name: "HelloWorld",
  components: {
    VChart
  },
  provide: {
    [THEME_KEY]: "dark"
  },
  setup: () => {
    const option = ref({
      title: {
        text: "频道消息汇总",
        left: "center"
      },
      tooltip: {
        trigger: "item",
        formatter: "{b} : {d}%"
      },
      legend: {
        orient: "vertical",
        left: "left",
        data: ["Direct", "Email", "Ad Networks", "Video Ads", "Search Engines"]
      },
      series: [
        {
          name: "频道消息汇总",
          type: "pie",
          radius: "55%",
          center: ["50%", "60%"],
          data: [
            { value: 1.50, name: "08:00" },
            { value: 3.43, name: "09:00" },
            { value: 5.40, name: "10:00" },
            { value: 5.65, name: "11:00" },
            { value: 7.39, name: "12:00" },
            { value: 5.97, name: "13:00" },
            { value: 4.59, name: "14:00" },
            { value: 5.95, name: "15:00" },
            { value: 4.48, name: "16:00" },
            { value: 7.25, name: "17:00" },
            { value: 5.64, name: "18:00" },
            { value: 4.83, name: "19:00" },
            { value: 5.62, name: "20:00" },
            { value: 6.28, name: "21:00" },
            { value: 5.99, name: "22:00" },
            { value: 7.15, name: "23:00" },
            { value: 5.70, name: "00:00" },
            { value: 2.35, name: "01:00" },
            { value: 0.58, name: "02:00" },
            { value: 0.41, name: "03:00" },
            { value: 0.15, name: "04:00" },
            { value: 0.18, name: "05:00" },
            { value: 1.00, name: "06:00" },
            { value: 2.54, name: "07:00" }
          ],
          emphasis: {
            itemStyle: {
              shadowBlur: 10,
              shadowOffsetX: 0,
              shadowColor: "rgba(0, 0, 0, 0.5)"
            }
          }
        }
      ]
    });

    return { option };
  }
});
</script>

<style scoped>
.chart {
  height: 400px;
}
</style>