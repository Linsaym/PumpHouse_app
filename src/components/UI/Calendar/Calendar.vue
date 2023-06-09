<template>
  <div class="calendar">
    <div class="calendar__header">Редактирование тарифа за месяц</div>
    <div class="calendar__year">
      <YearSlider @changeSelectedYear="changeSelectedYear"/>
    </div>
    <div class="calendar__main">
      <CalendarMonth v-for="(month,i) in months" :key="month" :month="month" :isSelected="i==selectedMonth"
                     :monthIndex="i"
                     :tariff="tariffs[i]" @changeSelectedMonth="changeSelectedMonth"/>
    </div>
    <div class="calendar__changeTariff">
      <!--      <input class="changeTariff__input" v-model="tariffOnSelectedMonth"/>-->
      <span contenteditable="true" class="changeTariff__input" @focusout="changeTariff">{{
          tariffOnSelectedMonth
        }}
      </span>
    </div>

  </div>
</template>

<script>
import CalendarMonth from "@/components/UI/Calendar/CalendarMonth.vue"
import YearSlider from "@/components/UI/Calendar/YearSlider.vue";

export default {
  name: "Calendar",
  components: {
    YearSlider,
    CalendarMonth
  },
  data() {
    return {
      selectedYear: 2023,
      selectedMonth: 0,
      months: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'],
      tariffs: [89, 231, 142, 42, 65, 87, 34, 65, 87, 56, 44, 65],
      tariffOnSelectedMonth: 0,
    }
  },
  methods: {
    changeSelectedMonth(selectedMonthIndex) {
      this.selectedMonth = selectedMonthIndex
      this.tariffOnSelectedMonth = this.tariffs[this.selectedMonth]
      this.changeSelectedDate()
    },
    changeTariff(event) {
      const newTariff = event.target.innerText
      this.tariffOnSelectedMonth = newTariff
      this.tariffs[this.selectedMonth] = newTariff
    },
    changeSelectedYear(year) {
      this.selectedYear = year
      this.changeSelectedDate()
    },
    changeSelectedDate() {
      this.$emit('changeSelectedDate', this.selectedYear, this.selectedMonth)
    }
  },
  mounted() {
    this.tariffOnSelectedMonth = this.tariffs[this.selectedMonth]
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
  justify-content: center;
  height: 52px;
  width: 360px;
  background: #1D1B20;
  border-bottom: 1px solid #49454F;
  text-align: center;
  font-size: 20px;
  line-height: 44px;
}

.changeTariff__input {
  display: block;
  margin-top: -2px;
  width: 360px;
  height: 52px;
  background: #333333;
  border-radius: 4px 4px 0px 0px;
  border: none;
  border-bottom: 1px solid #CAC4D0;

  font-family: 'Roboto';
  font-style: normal;
  font-weight: 400;
  font-size: 16px;
  line-height: 24px;
  padding-left: 32px;
  padding-top: 23px;
  color: #E6E0E9;
}

.changeTariff__input::after {
  content: " ₽";
}

.calendar__main > *:last-child {
  padding-bottom: 10px;
}

.changeTariff__input:focus {
  outline: none !important;
}

.calendar__changeTariff {
  position: relative;
}

.calendar__changeTariff::before {
  position: absolute;
  top: 3px;
  left: 32px;
  content: "Цена за 1м³ воды за выбранный месяц (в рублях)";
  font-family: 'Roboto';
  font-style: normal;
  font-weight: 400;
  font-size: 12px;
  line-height: 16px;
  color: #CAC4D0;

}

</style>