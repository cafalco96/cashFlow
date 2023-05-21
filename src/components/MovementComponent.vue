<template>
  <div class="cash-flow-layout--moviments-content--movements">
    <div class="cash-flow-layout--moviments-content--movements--description">
      <h3>{{ visualTitle }}</h3>
      <p>{{ visualDescription }}</p>
    </div>
    <div class="cash-flow-layout--moviments-content--movements--actions">
      <button @click="showModal = true">
        <img class="image-edit" src="../assets/edit-icon.svg" alt="edit" />
      </button>
      <Teleport to="#app">
        <ModalComponent v-show="showModal" @closeModal="showModal = false"
          ><form @submit.prevent="editMovement">
            <h2 class="cash-flow-layout--title">Add your movement</h2>
            <div class="cash-flow-layout--input-text">
              <label for="title">Title</label>
              <input
                type="text"
                name="title"
                id="title"
                v-model="titleEdited"
              />
            </div>
            <div class="cash-flow-layout--input-text">
              <label for="amount">Amount</label>
              <input
                type="number"
                name="amount"
                id="amount"
                v-model="amountEdited"
              />
            </div>
            <div class="cash-flow-layout--input-text__two-columns">
              <label for="description">Description</label>
              <textarea
                name="description"
                id="description"
                placeholder="Please, write a short description about Movement"
                v-model="descriptionEdited"
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
                  v-model="movementTypeEdited"
                />
              </div>
              <div class="cash-flow-layout--input-text__two-columns--radio">
                <label for="discharge">Spent</label>
                <input
                  type="radio"
                  name="movement-type"
                  id="discharge"
                  value="discharge"
                  v-model="movementTypeEdited"
                />
              </div>
            </div>
            <div class="cash-flow-layout--content-button">
              <button class="cash-flow-layout--button">Edit Movement</button>
            </div>
          </form></ModalComponent
        >
      </Teleport>
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
import { ref, reactive, defineProps, computed, defineEmits } from "vue";
import ModalComponent from "./ModalComponent.vue";
const showModal = ref(false);
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
});
const { title, description, amount, id } = reactive(props);
const titleEdited = ref(title);
const descriptionEdited = ref(description);
let amountEdited = ref(amount);
if (Math.sign(amount) === -1) {
  amountEdited = ref(-amount);
  console.log(amountEdited.value, amount);
}
console.log(amountEdited.value, amount);
const movementTypeEdited = ref("income");
const deleteMovement = () => emit("deleteMovement", id);
const currencyAmount = computed(() =>
  currencyAmountFormat.format(visualAmount)
);
const editMovement = () => {
  showModal.value = false;
  emit("editMovement", id, {
    title: titleEdited.value,
    description: descriptionEdited.value,
    amount:
      movementTypeEdited.value === "income"
        ? amountEdited.value
        : -amountEdited.value,
    time: new Date(),
    id: new Date().getTime(),
  });
};
const emit = defineEmits(["deleteMovement", "editMovement"]);
const isDischarge = computed(() => amount < 0);
const visualTitle = titleEdited.value ? titleEdited.value : title;
const visualDescription = descriptionEdited.value
  ? descriptionEdited.value
  : description;
const visualAmount = amount;
</script>
<style lang="scss">
.income {
  color: #30bd9c;
}

.discharge {
  color: red;
}
</style>
