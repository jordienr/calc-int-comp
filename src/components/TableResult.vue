<template>
  <div class="table-result">
    <div class="row headers">
      <span class="year">Año</span>
      <span class="contribution">Contribuciones</span>
      <span class="interest">Ganancias por interés</span>
      <span class="total">Total año</span>
    </div>
    <div class="row" v-for="year in years">
      <span class="year">{{ year.year }}</span>
      <span class="contribution">{{ year.deposits }}</span>
      <span class="interest">{{ year.interestGain }}</span>
      <span class="total">{{ year.yearTotal }}</span>
    </div>
    <div class="row">
      <span><b>Total: </b>{{ total }}</span>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from "vue";

export default defineComponent({
  name: "TableResult",
  components: {},
  props: {
    total: {
      required: true,
      type: String,
    },
    years: {
      required: true,
      type: Array,
    },
  },
});
</script>

<style lang="scss" scoped>
.table-result {
  overflow-x: auto;
  max-width: 600px;
}
.row {
  display: flex;
  border-bottom: 1px solid #eaeaea;
  * {
    padding: 0.5rem;
  }
  &.headers {
    font-weight: 700;
    font-size: 10px;
  }
  .year {
    width: 10%;
  }
  .contribution {
    width: 25%;
  }
  .interest {
    width: 25%;
  }
  .total {
    width: 15%;
  }
}
</style>
