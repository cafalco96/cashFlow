<template>
  <div class="cash-flow-layout--modal">
    <div class="cash-flow-layout--modal-header">
      <p>New Movement</p>
      <button @click="closeModal">
        <img src="../assets/close-icon.svg" alt="close" />
      </button>
    </div>
    <div class="cash-flow-layout--modal-body">
      <slot></slot>
    </div>
  </div>
</template>
<script setup>
import { defineEmits } from "vue";
const emit = defineEmits(["closeModal"]);
const closeModal = () => emit("closeModal");
</script>
<style lang="scss">
.cash-flow-layout--modal {
  z-index: 2;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background-color: white;
  &-header {
    display: flex;
    justify-content: space-between;
    button {
      background: none;
      border: 0;
      color: inherit;
      line-height: normal;
      overflow: visible;
      padding: 0;
    }
    p {
      color: #30a8bd;
      font-size: 25px;
    }
  }
  &-body {
    display: flex;
    justify-content: center;
    padding: 100px;
  }
}
</style>
