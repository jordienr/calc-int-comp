<template>
  <div class="ci-form-container">
    <div class="input-group">
      <label for="investment">Inversión incial</label>
      <input
        type="number"
        name="investment"
        id="investment"
        inputmode="decimal"
        v-model="investment"
      />
    </div>
    <div class="input-group">
      <label for="contribution">Contribución mensual</label>
      <input
        type="number"
        name="contribution"
        id="contribution"
        inputmode="decimal"
        v-model="contribution"
      />
    </div>
    <div class="d-flex">
      <div class="input-group">
        <label for="interest">Interés estimado</label>
        <input
          type="number"
          name="interest"
          id="interest"
          v-model="interest"
          inputmode="decimal"
        />
      </div>
      <div class="input-group">
        <label for="length">Duración en años</label>
        <input
          type="number"
          name="length"
          id="length"
          v-model="length"
          inputmode="decimal"
        />
      </div>
    </div>
    <!-- <div class="input-group">
      <label for="frequency">Frecuencia</label>
      <div class="d-flex">
        <div class="input-radio-group">
          <input
            type="radio"
            name="monthly-frequency"
            id="monthly"
            value="monthly"
            v-model="frequency"
          />
          <label for="monthly">Mensual</label>
        </div>
        <div class="input-radio-group">
          <input
            type="radio"
            name="yearly-frequency"
            id="yearly"
            value="yearly"
            v-model="frequency"
          />
          <label for="yearly">Anual</label>
        </div>
      </div>
    </div> -->
    <footer class="actions">
      <button class="btn btn-text" @click="reset">Borrar</button>
      <button class="btn btn-primary" @click="calc">Calcular</button>
    </footer>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from "vue";
export default defineComponent({
  name: "CIForm",
  props: {},
  data() {
    return {
      investment: 1000,
      contribution: 100,
      interest: 10,
      length: 10,
      frequency: "monthly",
    };
  },
  computed: {
    formData(): object {
      return {
        investment: this.investment,
        contribution: this.contribution,
        interest: this.interest,
        frequency: this.frequency,
        length: this.length,
      };
    },
  },
  methods: {
    reset() {
      this.investment = 0;
      this.contribution = 0;
      this.interest = 0;
      this.frequency = "monthly";
      this.length = 0;
    },
    calc() {
      this.$emit("calc", this.formData);
    },
  },
});
</script>

<style lang="scss" scoped>
.ci-form-container {
  padding: 0.3rem;
}
.input-group {
  display: flex;
  flex-direction: column;
  margin-top: 1rem;
  label {
    font-weight: 500;
  }
  input {
    padding: 6px;
    display: block;
    border-radius: 4px;
    margin-top: 2px;
    border: 1px solid #ddd;
  }
}

.input-radio-group {
  display: flex;
  align-items: center;
  padding: 0.5rem;
  input {
    margin: 0;
    margin-right: 0.5rem;
  }
}
</style>
