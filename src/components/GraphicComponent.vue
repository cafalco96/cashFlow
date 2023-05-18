<template>
  <div>
    <svg
      @touchstart="tap"
      @touchmove="tap"
      @touchend="untap"
      @mousedown="clickEl"
      @mouseup="unclickEl"
      viewBox="0 0 300 200"
    >
      <line
        stroke="#c4c4c4"
        stroke-width="2"
        x1="0"
        :y1="zero"
        x2="300"
        :y2="zero"
      ></line>
      <polyline
        fill="none"
        stroke="#30a8bd"
        stroke-width="2"
        :points="points"
      ></polyline>
      <line
        v-show="showPointer"
        stroke="#30bd9c"
        stroke-width="2"
        :x1="pointer"
        y1="0"
        :x2="pointer"
        y2="200"
      ></line>
    </svg>
    <span>Last 30 days</span>
  </div>
</template>
<script setup>
import { defineProps, toRefs, computed, ref, defineEmits, watch } from "vue";
const props = defineProps({
  amounts: {
    type: Array,
    default: () => [],
  },
});
const { amounts } = toRefs(props);
const amountToPixels = (amount) => {
  const min = Math.min(...amounts.value);
  const max = Math.max(...amounts.value);
  const amountAbs = amount + Math.abs(min);
  const minmax = Math.abs(max) + Math.abs(min);
  return 200 - ((amountAbs * 100) / minmax) * 2;
};
const zero = computed(() => {
  return amountToPixels(0);
});
const points = computed(() => {
  const total = amounts.value.length;
  return amounts.value.reduce((points, amount, i) => {
    const x = (300 / total) * (i + 1);
    const y = amountToPixels(amount);
    return `${points} ${x},${y}`;
  }, `0, ${amountToPixels(amounts.value.length ? amounts.value[0] : 0)}`);
});
const showPointer = ref(false);
const emit = defineEmits(["select"]);
const pointer = ref(0);
watch(pointer, (value) => {
  const index = Math.ceil(value / (300 / amounts.value.length));
  if (index < 0 || index > amounts.value.length) return;
  emit("select", amounts.value[index - 1], index);
});
const tap = ({ target, touches }) => {
  showPointer.value = true;
  const elWidth = target.getBoundingClientRect().width;
  const elX = target.getBoundingClientRect().x;
  const touchX = touches[0].clientX;
  pointer.value = ((touchX - elX) * 300) / elWidth;
};
const untap = () => (showPointer.value = false);
const unclickEl = () => (showPointer.value = false);
const clickEl = (e) => {
  showPointer.value = true;
  const elWidth = e.target.getBoundingClientRect().width;
  const elX = e.target.getBoundingClientRect().x;
  const clickX = e.clientX;
  pointer.value = ((clickX - elX) * 300) / elWidth;
};
</script>
<style lang="scss" scoped>
span {
  text-align: center;
}
svg {
  width: 100%;
}
</style>
