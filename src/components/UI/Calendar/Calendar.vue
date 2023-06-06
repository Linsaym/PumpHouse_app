<template>
  <div class="calendar">
    <div class="calendar__header">Редактирование тарифа за месяц</div>
    <div class="calendar__year">
      <span>2023</span>
    </div>
    <div class="calendar__main">
      <CalendarMonth/>
    </div>
  </div>
</template>

<script>
import CalendarMonth from "@/components/UI/Calendar/CalendarMonth.vue"

export default {
  name: "Calendar",
  components: {
    CalendarMonth
  },
  data() {
    return {
      selectedYear: 2023,
      selectedMonth: 1,
      periods: [
        {}
      ]
    }
  }
}
</script>
<style>
.calendar__header {
  border-radius: 20px 20px 0px 0px;
  width: 360px;
  height: 48px;
  background: #49454F;
  font-family: 'Roboto';
  font-style: normal;
  font-weight: 400;
  font-size: 20px;
  line-height: 46px;
  text-align: center;
}

.calendar__year {
  display: flex;
  height: 52px;
  width: 360px;
  background: #1D1B20;
  border-bottom: 1px solid #49454F;
  text-align: center;
  font-size: 20px;
  line-height: 44px;
}

.calendar__main {

}

</style>