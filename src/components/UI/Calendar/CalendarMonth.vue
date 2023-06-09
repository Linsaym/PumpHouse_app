<template>
  <div @click="$emit('changeSelectedMonth',monthIndex)" class="calendar__month-block"
       :class="{'month-block--selected':isSelected}">
    <div class="calendar__month-name">{{ month }}</div>
    <div @click="$emit('changeInputOnIndications',monthIndex)" class="month__indications">{{ indications }}м³</div>
    <div @click="$emit('changeInputOnTariff',monthIndex)" class="month__tariff">{{ tariff }}₽</div>
  </div>
</template>
<script>
export default {
  props: {
    month: String,
    monthIndex: Number,
    isSelected: Boolean,
    tariff: Number | String,
    indications: Number | String,
  },
  // data() {
  //   return {
  //     isSelected: false,
  //   }
  // }
}
</script>
<style>
.calendar__month-block {
  cursor: pointer;
  background: #1D1B20;
  height: 42px;
  display: grid;
  grid-template-columns: 70px 130px 90px;
  align-items: center;
  padding-left: 42px;
}

.month__tariff, .month__indications {
  text-align: center;
  width: 70px;
  height: 21px;
  background: #49454F;
  border-radius: 11px;
}

.month__indications {
  margin-left: 22px;
  width: 90px;
}

.month-block--selected {
  background: rgba(230, 224, 233, 0.08);
}
</style>