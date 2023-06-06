<script setup>

import ResidentsList from "@/components/UI/ResidentsList.vue";
import Calendar from "@/components/UI/Calendar/Calendar.vue";
</script>

<template>
  <div class="wrapper homePage">
    <ResidentsList/>
    <Calendar/>
  </div>
</template>

<style>
.wrapper {
  max-width: 1320px;
  display: block;
  margin: 0 auto;
  padding: 40px;
}

.homePage {
  display: flex;
  justify-content: space-between;
}
</style>
