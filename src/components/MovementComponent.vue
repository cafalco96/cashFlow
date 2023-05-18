<template>
  <div class="cash-flow-layout--moviments-content--movements">
    <div class="cash-flow-layout--moviments-content--movements--description">
      <h3>{{ title }}</h3>
      <p>{{ description }}</p>
    </div>
    <div class="cash-flow-layout--moviments-content--movements--actions">
      <button @click="deleteMovement">
        <img src="../assets/trash-icon.svg" alt="delete" />
      </button>
      <p :class="[{ discharge: isDischarge, income: !isDischarge }]">
        {{ currencyAmount }}
      </p>
    </div>
  </div>
</template>
<script setup>
import { toRefs, defineProps, computed, defineEmits } from "vue";
const currencyAmountFormat = new Intl.NumberFormat("en-US", {
  style: "currency",
  currency: "USD",
});
const props = defineProps({
  title: {
    type: String,
  },
  description: {
    type: String,
  },
  amount: {
    type: Number,
  },
  id: {
    type: Number,
  },
  time: {},
});
const { title, description, amount, id } = toRefs(props);
const deleteMovement = () => emit("deleteMovement", id.value);
const currencyAmount = computed(() =>
  currencyAmountFormat.format(amount.value)
);
const emit = defineEmits(["deleteMovement"]);
const isDischarge = computed(() => amount.value < 0);
</script>
<style scoped>
.income {
  color: green;
}
.discharge {
  color: red;
}
</style>
