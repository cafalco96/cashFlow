<template>
  <h2 class="cash-flow-layout--moviments-content--title__secondary">
    Move<span class="cash-flow-layout--moviments-content--title__secondary"
      >ments</span
    >
  </h2>
  <div>
    <MovementComponent
      v-for="movement in movements"
      :key="movement.id"
      :title="movement.title"
      :description="movement.description"
      :amount="movement.amount"
      :id="movement.id"
      :time="movement.time"
      @delete-movement="remove"
      @edit-movement="edit"
    >
    </MovementComponent>
  </div>
</template>
<script setup>
import { toRefs, defineProps, defineEmits } from "vue";
import MovementComponent from "./MovementComponent.vue";
const props = defineProps({
  movements: {
    type: Array,
    default: () => [],
  },
});
const emit = defineEmits(["delete-movement", "edit-movement"]);
const { movements } = toRefs(props);
const remove = (id) => {
  emit("delete-movement", id);
};
const edit = (id, movement) => {
  emit("edit-movement", id, movement);
};
</script>
