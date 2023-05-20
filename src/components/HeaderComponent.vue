<template>
  <header class="cash-flow-layout--header">
    <button @click="reload">
      <img src="../assets/money-svgrepo-com.svg" alt="logo" />
    </button>
    <h2><span>Cash</span>Flow</h2>
    <span></span>
  </header>
</template>
<script>
export default {
  methods: {
    reload() {
      location.reload();
    },
  },
};
</script>
<style lang="scss" scope>
button {
  height: 57px;
  background: none;
  border: 0;
  color: inherit;
  line-height: normal;
  overflow: visible;
  padding: 0;
}
</style>
