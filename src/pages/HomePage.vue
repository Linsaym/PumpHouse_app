<script setup>

import ResidentsList from "@/components/UI/ResidentsList.vue";
import Calendar from "@/components/UI/Calendar.vue";
</script>

<template>
  <div class="wrapper" style="display: flex">
    <ResidentsList/>
    <Calendar/>
  </div>
</template>

<style scoped>
.wrapper {
  max-width: 1320px;
  display: block;
  margin: 0 auto;
  padding: 40px;
}
</style>
