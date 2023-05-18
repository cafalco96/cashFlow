<template>
  <header class="cash-flow-layout--header">
    <img src="../assets/money-svgrepo-com.svg" alt="logo" @click="reload" />
    <h2><span>Cash</span>Flow</h2>
    <span></span>
  </header>
</template>
<script>
export default {
  methods: {
    reload() {
      location.reload;
    },
  },
};
</script>
