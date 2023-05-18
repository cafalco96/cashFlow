<template>
  <LayoutComponent>
    <template #header>
      <HeaderComponent></HeaderComponent>
    </template>
    <template #frontcontent>
      <FrontComponent
        :label="label"
        :amount="amount"
        :total-amount="totalAmount"
      >
        <template #graphic>
          <GraphicComponent
            :amounts="amounts"
            @select="select"
          ></GraphicComponent>
        </template>
        <template #action>
          <ActionComponent
            v-on:create-movement="createMovement"
          ></ActionComponent>
        </template>
      </FrontComponent>
    </template>
    <template #movimentsComponent>
      <MovimentsComponent
        :movements="movements"
        @delete-movement="deleteMoviment"
      ></MovimentsComponent>
    </template>
  </LayoutComponent>
</template>
<script>
import LayoutComponent from "./LayoutComponent.vue";
import HeaderComponent from "./HeaderComponent.vue";
import FrontComponent from "./FrontComponent.vue";
import MovimentsComponent from "./MovimentsComponent.vue";
import ActionComponent from "./ActionComponent.vue";
import GraphicComponent from "./GraphicComponent.vue";
export default {
  components: {
    LayoutComponent,
    HeaderComponent,
    FrontComponent,
    MovimentsComponent,
    ActionComponent,
    GraphicComponent,
  },
  data() {
    return {
      amount: null,
      label: null,
      movements: [],
    };
  },
  computed: {
    amounts() {
      const lastDays = this.movements
        .filter((m) => {
          const today = new Date();
          const oldDate = today.setDate(today.getDate() - 30);
          return m.time > oldDate;
        })
        .map((m) => m.amount);
      return lastDays.map((m, i) => {
        const lastMovements = lastDays.slice(0, i);
        return lastMovements.reduce((suma, movement) => {
          return suma + movement;
        }, 0);
      });
    },
    totalAmount() {
      return this.movements.reduce((suma, m) => {
        return suma + m.amount;
      }, 0);
    },
  },
  mounted() {
    const movements = JSON.parse(localStorage.getItem("movements"));
    if (Array.isArray(movements)) {
      this.movements = movements.map((m) => {
        return { ...m, time: new Date(m.time) };
      });
    }
  },

  methods: {
    createMovement(movement) {
      this.movements.push(movement);
      this.save();
    },
    deleteMoviment(id) {
      const index = this.movements.findIndex((m) => m.id === id);
      this.movements.splice(index, 1);
      this.save();
    },
    save() {
      localStorage.setItem("movements", JSON.stringify(this.movements));
    },
    select(el, index) {
      this.amount = el;
      if (index < 0 || index > this.amounts.length) return;
      const dateString = this.movements[index].time.toLocaleString("en-US", {
        day: "2-digit",
        month: "2-digit",
        year: "numeric",
      });
      this.label = dateString;
    },
  },
};
</script>
