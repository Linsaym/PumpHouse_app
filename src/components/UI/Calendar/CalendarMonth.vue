<template>
  <div class="calendar__month-block">
    <div class="calendar__month-name"></div>
    <div class="month__tariff"></div>
  </div>
</template>
<script>
export default {
  props: {
    periods: Array,
  }
}
</script>
<style>
.calendar__month {
  display: grid;
  grid-template-columns: 200px;
  padding-left: 52px;
}
</style>