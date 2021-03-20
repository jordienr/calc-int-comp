<template>
  <div class="chart-result">
    <apexchart
      width="600px"
      type="line"
      :options.sync="chartOptions"
      :series="series"
    ></apexchart>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from "vue";
import VueApexCharts from "vue3-apexcharts";

export default defineComponent({
  name: "ChartResult",
  components: {
    apexchart: VueApexCharts,
  },
  props: {
    chartOptions: {
      required: true,
      type: Object,
    },
    series: {
      required: true,
      type: Array,
    },
  },
});
</script>

<style lang="scss" scoped>
.chart-result {
  overflow-x: auto;
  padding-right: 2rem;
}
</style>
