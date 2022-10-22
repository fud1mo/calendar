<template>
  <CalendarModal />
</template>

<script>
import CalendarModal from "./view/CalendarModal.vue";

export default {
  name: "App",
  components: {
    CalendarModal,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
