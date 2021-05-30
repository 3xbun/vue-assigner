<template>
  <div id="home">
    <Calendar />
    <Homework />
  </div>
</template>

<script>
import Calendar from "../components/Calendar";
import Homework from "../components/Homework";

export default {
  name: "Home",
  components: {
    Calendar,
    Homework,
  },
};
</script>

<style>
#home {
  margin-top: 3em;
}
</style>
