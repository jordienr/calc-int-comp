<template>
  <div class="container">
    <div class="result">
      <h3>Resultado</h3>
      <div class="select-result-type">
        <button
          class="btn"
          :class="{
            'btn-primary': resultToShow === 'summary',
            'btn-text': resultToShow !== 'summary',
          }"
          @click="resultToShow = 'summary'"
        >
          Resumen
        </button>
        <button
          class="btn"
          :class="{
            'btn-primary': resultToShow === 'graph',
            'btn-text': resultToShow !== 'graph',
          }"
          @click="resultToShow = 'graph'"
        >
          Gráfica
        </button>
        <button
          class="btn"
          :class="{
            'btn-primary': resultToShow === 'table',
            'btn-text': resultToShow !== 'table',
          }"
          @click="resultToShow = 'table'"
        >
          Tabla
        </button>
      </div>

      <div class="result">
        <div v-if="itsEmpty">
          <h2>Empieza haciendo un cálculo</h2>
        </div>
        <div v-else class="results-container">
          <SummaryResult
            :total="total"
            :totalContributions="totalContributions"
            :totalInterest="totalInterest"
          />
          <ChartResult
            v-if="resultToShow === 'graph'"
            :chartOptions="chartOptions"
            :series="series"
          />
          <TableResult
            v-if="resultToShow === 'table'"
            :years="years"
            :total="total"
          />
        </div>
      </div>
    </div>
    <div class="ci-form">
      <div class="d-flex calc-title">
        <h3>Calculadora</h3>
        <button class="btn btn-text" @click="showCalc = !showCalc">
          <Icon v-if="showCalc" :icon="chevronDown"></Icon>
          <Icon v-if="!showCalc" :icon="chevronUp"></Icon>
        </button>
      </div>
      <CIForm v-if="showCalc" @calc="calculate"></CIForm>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import CIForm from "./components/CIForm.vue";
import ChartResult from "./components/ChartResult.vue";
import TableResult from "./components/TableResult.vue";
import Icon from "./components/Icon.vue";
import SummaryResult from "./components/SummaryResult.vue";
import { chevronDown, chevronUp } from "./assets/icons";

interface FormData {
  investment: number;
  contribution: number;
  interest: number;
  length: number;
}

interface Year {
  year: number;
  deposits: number;
  interestGain: number;
  yearTotal: number;
}

export default defineComponent({
  name: "App",
  components: {
    CIForm,
    ChartResult,
    TableResult,
    Icon,
    SummaryResult,
  },
  data() {
    return {
      chevronDown,
      chevronUp,
      itsEmpty: true,
      xAxis: [] as string[],
      resultToShow: "summary",
      total: "",
      totalContributions: 0,
      totalInterest: 0,
      years: [] as {}[],
      showCalc: true,
      seriesInterest: [] as number[],
      seriesContributions: [] as number[],
    };
  },
  methods: {
    calculate(data: FormData) {
      const { investment, contribution, interest, length } = data;

      let years = [];
      let total = +investment;
      let totalContributions = +investment;
      let totalInterest = 0;

      for (let i = 0; i < length; i++) {
        const year = i + 1;
        const deposits = +contribution * 12;
        const interestGain = ((+total + +deposits) * +interest) / 100;
        const yearTotal = +deposits + +interestGain;

        totalInterest += interestGain;

        this.xAxis.push("Año " + year);
        total += +yearTotal;
        totalContributions += deposits;

        this.seriesContributions.push(+totalContributions.toFixed(2));
        this.seriesInterest.push(+total.toFixed(2));

        years.push({
          year: year.toFixed(0),
          deposits: deposits.toFixed(2),
          interestGain: interestGain.toFixed(2),
          yearTotal: yearTotal.toFixed(2),
        });
      }

      console.log({ total, years });
      this.total = total.toFixed(2);
      this.years = years;
      this.totalContributions = totalContributions;
      this.totalInterest = totalInterest.toFixed(2);
      this.itsEmpty = false;
    },
  },
  computed: {
    chartOptions() {
      return {
        width: "100%",
        xaxis: {
          categories: this.xAxis,
        },
      };
    },
    series() {
      return [
        {
          name: "Total + interés",
          data: this.seriesInterest,
        },
        {
          name: "Total Contribuciones",
          data: this.seriesContributions,
        },
      ];
    },
  },
});
</script>

<style lang="scss">
@import "./assets/style.scss";

body {
  padding: 0;
  margin: 0;
  background-color: #f1f5f9;
}
#app {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: 6px;
}
</style>

<style lang="scss" scoped>
.ci-form {
  border-top: 1px solid #e2e8f0;
}
.container {
  display: flex;
  flex-direction: column;
  .result {
    flex-grow: 1;
  }
}
.selected {
  font-weight: bold;
  background-color: red;
}
.result {
  margin-top: 2rem;
}
.result-container {
  margin-bottom: 320px;
  overflow-x: auto;
}

.ci-form {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: #f1f5f9;
  padding-bottom: 2.5rem;
}

.calc-title {
  justify-content: space-between;
}

.select-result-type {
  border-bottom: 1px solid #ddd;
  padding-bottom: 0.5rem;
}
</style>
