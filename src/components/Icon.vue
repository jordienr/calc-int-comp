<template>
  <div class="icon-container">
    <div v-html="icon"></div>
  </div>
</template>
<script lang="ts">
import { ref, defineComponent } from "vue";

export default defineComponent({
  name: "Icon",
  props: {
    icon: {
      required: true,
      type: String,
    },
  },
});
</script>

<style lang="scss" scoped>
.icon-container {
  width: 20px;
}
</style>
