<template>
  <button class="cash-flow-layout--button" @click="showModal = true">
    <span>Add</span> Movement
  </button>
  <Teleport to="#app">
    <ModalComponent v-show="showModal" @closeModal="showModal = false"
      ><form @submit.prevent="addMovement">
        <h2 class="cash-flow-layout--title">Add your movement</h2>
        <div class="cash-flow-layout--input-text">
          <label for="title">Title</label>
          <input type="text" name="title" id="title" v-model="title" />
        </div>
        <div class="cash-flow-layout--input-text">
          <label for="amount">Amount</label>
          <input type="number" name="amount" id="amount" v-model="amount" />
        </div>
        <div class="cash-flow-layout--input-text__two-columns">
          <label for="description">Description</label>
          <textarea
            name="description"
            id="description"
            placeholder="Please, write a short description about Movement"
            v-model="description"
          />
        </div>
        <div class="cash-flow-layout--input-text__two-columns">
          <span> Movement Type </span>
          <div class="cash-flow-layout--input-text__two-columns--radio">
            <label for="income">Income</label>
            <input
              type="radio"
              name="movement-type"
              id="income"
              value="income"
              v-model="movementType"
            />
          </div>
          <div class="cash-flow-layout--input-text__two-columns--radio">
            <label for="discharge">Discharge</label>
            <input
              type="radio"
              name="movement-type"
              id="discharge"
              value="discharge"
              v-model="movementType"
            />
          </div>
        </div>
        <div class="cash-flow-layout--content-button">
          <button class="cash-flow-layout--button">Add Movement</button>
        </div>
      </form></ModalComponent
    >
  </Teleport>
</template>
<script setup>
import { ref, defineEmits } from "vue";
import ModalComponent from "./ModalComponent.vue";
const showModal = ref(false);
const title = ref("");
const amount = ref(0);
const description = ref("");
const movementType = ref("income");
const emit = defineEmits(["createMovement"]);
const addMovement = () => {
  showModal.value = false;
  emit("createMovement", {
    title: title.value,
    description: description.value,
    amount: movementType.value === "income" ? amount.value : -amount.value,
    time: new Date(),
    id: new Date().getTime(),
  });
  title.value = "";
  description.value = "";
  amount.value = 0;
};
</script>
<style lang="scss">
.cash-flow-layout--button {
  margin-top: 20px;
  border-color: #30a8bd;
  color: #fff;
  box-shadow: 0 0 40px 40px #30a8bd inset, 0 0 0 0 #30a8bd;
  -webkit-transition: all 150ms ease-in-out;
  transition: all 150ms ease-in-out;
  height: 50px;
  width: 150px;
  border-radius: 25px;
  &:hover {
    box-shadow: 0 0 10px 0 #30a8bd inset, 0 0 10px 4px #30a8bd;
    color: #30a8bd;
  }
}
.cash-flow-layout--input-text {
  display: flex;
  justify-content: space-between;
  margin: 20px;
  flex-direction: column;
  @media (min-width: 992px) {
    flex-direction: row;
    align-items: center;
  }
  &__two-columns {
    display: flex;
    text-align: center;
    flex-direction: column;
    margin: 20px;
    textarea {
      height: 65px;
      margin-top: 20px;
      border-color: #30a8bd;

      &:focus {
        outline: none;
        border-color: #30a8bd;
        box-shadow: 0 0 5px #30a8bd;
      }
    }
    span {
      margin-bottom: 20px;
    }
    &--radio {
      display: flex;
      justify-content: center;
    }
  }
  input[type="number"] {
    text-align: end;
    border-color: #30a8bd;
    height: 30px;

    &:focus {
      outline: none;
      border-color: #30a8bd;
      box-shadow: 0 0 5px #30a8bd;
    }
  }
  input[type="text"] {
    border-color: #30a8bd;
    height: 30px;

    &:focus {
      outline: none;
      border-color: #30a8bd;
      box-shadow: 0 0 5px #30a8bd;
    }
  }
}
.cash-flow-layout--title {
  text-align: center;
}
.cash-flow-layout--content-button {
  display: flex;
  justify-content: center;
}
</style>
