<template>
  <main>
    <p>{{ visualLabel }}</p>
    <h2 class="amount">{{ currencyAmount }}</h2>
    <div class="cash-flow-layout--resume--graphic">
      <slot name="graphic"></slot>
    </div>
    <div class="cash-flow-layout--resume--button-action">
      <slot name="action"></slot>
    </div>
  </main>
</template>
<script>
const currencyAmountFormat = new Intl.NumberFormat("en-US", {
  style: "currency",
  currency: "USD",
});
export default {
  props: {
    label: {
      type: String,
      default: null,
    },
    defaultLabel: {
      typeof: String,
      default: "Total Amount",
    },
    amount: {
      type: Number,
      default: null,
    },
    totalAmount: {
      type: Number,
    },
  },
  computed: {
    visualAmount() {
      return this.amount !== null ? this.amount : this.totalAmount;
    },
    visualLabel() {
      return this.label !== null ? this.label : this.defaultLabel;
    },
    currencyAmount() {
      return currencyAmountFormat.format(this.visualAmount);
    },
  },
};
</script>
<style lang="scss">
.amount {
  font-size: 50px;
  color: #30bd9c;
}
</style>
