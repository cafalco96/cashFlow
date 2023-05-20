<template>
  <div class="cash-flow-layout">
    <slot name="header"></slot>
    <div class="cash-flow-layout--container">
      <div class="cash-flow-layout--resume">
        <slot name="frontcontent"></slot>
      </div>
      <div class="cash-flow-layout--moviments-container">
        <div
          class="cash-flow-layout--moviments-container--header"
          @click="showMovements = !showMovements"
        >
          <div
            class="cash-flow-layout--moviments-container--header__grip"
          ></div>
          <h2>Record</h2>
        </div>
        <div class="cash-flow-layout--moviments-content" v-show="showMovements">
          <slot name="movimentsComponent"></slot>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref } from "vue";
const showMovements = ref(false);
</script>
<style lang="scss">
.cash-flow-layout {
  box-sizing: border-box;
  &--header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: fixed;
    width: 100%;
    img {
      width: 50px;
    }
    h2 {
      color: #30a8bd;
    }
    span {
      color: #30bd9c;
      width: 50px;
    }
  }
  &--container {
    position: absolute;
    width: 100%;
  }
  &--resume {
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding-top: 100px;
    height: 100vh;
  }
  &--moviments-container {
    position: absolute;
    bottom: 0;
    width: 100%;
    background-color: white;
    box-shadow: 0 -8px 16px #e5e5e5;
    border-radius: 24px;
    &--header {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      padding: 24px;
      width: 100%;
      box-sizing: border-box;
      color: #30a8bd;
      &__grip {
        width: 120px;
        height: 8px;
        background-color: #30bd9c;
        border-radius: 4px;
      }
    }
  }
  &--moviments-content {
    height: 75vh;
    width: 100%;

    &--title {
      color: #30a8bd;
      text-align: center;

      &__secondary {
        color: #30bd9c;
        text-align: center;
      }
    }
    &--movements {
      display: grid;
      grid-template-columns: 1fr 1fr;
      border-radius: 60px 20px 5px 90px;
      padding: 10px;
      overflow: hidden;
      &--description {
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        justify-content: center;
        background-color: #e0f6fa;
        padding-left: 25px;
      }
      &--actions {
        display: flex;
        flex-direction: column;
        align-items: flex-end;
        justify-content: center;
        background-color: #e0f6fa;
        padding-right: 25px;
        button {
          height: 57px;
          background: none;
          border: 0;
          color: inherit;
          line-height: normal;
          overflow: visible;
          padding: 0;
        }
      }
    }
  }
}
</style>
