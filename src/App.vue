<template>
  <Suspense>
    <template #default>
      <Home></Home>
    </template>
    <template #fallback>
      <SplashScreen></SplashScreen>
    </template>
  </Suspense>
</template>

<script>
import { defineAsyncComponent } from "vue";
import SplashScreen from "./components/SplashScreen.vue";
export default {
  components: {
    Home: defineAsyncComponent(
      () =>
        new Promise((resolve) => {
          setTimeout(() => {
            resolve(import("./components/MainMenu.vue"));
          }, 2500);
        })
    ),
    SplashScreen,
  },
};
</script>

<style lang="scss">
.app {
  min-height: 100vh;
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
}
</style>
