<template>
  <div class="summary-result">
    <div class="row">
      <span><b>Total: </b> {{ total }}</span>
    </div>
    <div class="row">
      <span><b>Total contribuciones: </b> {{ totalContributions }}</span>
    </div>
    <div class="row">
      <span><b>Total Intereses: </b> {{ totalInterest }}</span>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from "vue";

export default defineComponent({
  name: "SummaryResult",
  components: {},
  props: {
    total: {
      required: true,
      type: String,
    },
    totalInterest: {
      required: true,
      type: String,
    },
    totalContributions: {
      required: true,
      type: String,
    },
  },
});
</script>

<style lang="scss" scoped>
.summary-result {
  margin-top: 1rem;
}
.row {
  padding: 0.5rem;
}
</style>
