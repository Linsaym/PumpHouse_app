<template>
  <div class="calendar">
    <div class="calendar__header">Редактирование тарифа за месяц</div>
    <div class="calendar__year">
      <YearSlider @changeSelectedYear="changeSelectedYear"/>
    </div>
    <div class="calendar__main">
      <CalendarMonth v-for="(month,i) in months" :key="month" :month="month" :isSelected="i==selectedMonth"
                     :monthIndex="i"
                     :indications="indications[i]"
                     :tariff="tariffs[i]" @changeSelectedMonth="changeSelectedMonth"
                     @changeInputOnIndications="changeInputOnIndications"
                     @changeInputOnTariff="changeInputOnTariff"
      />
    </div>
    <div v-if="inputType=='Tariff'" class="calendar__changeTariff">
      <span contenteditable="true" class="changeTariff__input" @focusout="changeTariff">{{
          tariffOnSelectedMonth
        }}
      </span>
    </div>
    <div v-if="inputType=='Indications'" class="calendar__changeIndications">
      <span contenteditable="true" class="changeIndications__input"
            @focusout="changeIndications">{{
          IndicationsOnSelectedMonth
        }}
      </span>
    </div>
    <div v-if="inputType==''" class="calendar__footer"></div>

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
      inputType: '',
      selectedYear: 2023,
      selectedMonth: null,
      months: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'],
      tariffs: [100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 110, 111],
      indications: [110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 12100],
      tariffOnSelectedMonth: 0,
      IndicationsOnSelectedMonth: 0,
    }
  },
  methods: {
    changeInputOnTariff() {
      this.inputType = 'Tariff'
    },
    changeInputOnIndications() {
      this.inputType = 'Indications'
    },
    changeSelectedMonth(selectedMonthIndex) {
      this.selectedMonth = selectedMonthIndex
      this.tariffOnSelectedMonth = this.tariffs[this.selectedMonth]
      this.IndicationsOnSelectedMonth = this.indications[this.selectedMonth]
      this.changeSelectedDate()
    },
    changeTariff(event) {
      const newTariff = event.target.innerText
      this.tariffOnSelectedMonth = newTariff
      this.tariffs[this.selectedMonth] = newTariff
    },
    changeIndications(event) {
      const newIndications = event.target.innerText
      this.IndicationsOnSelectedMonth = newIndications
      this.indications[this.selectedMonth] = newIndications
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
.calendar__footer {
  border-radius: 0px 0px 20px 20px;
  width: 360px;
  height: 48px;
  background: #49454F;

}

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

.changeTariff__input, .changeIndications__input {
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

.changeIndications__input::after {
  content: " м³";
}

.calendar__main > *:last-child {
  padding-bottom: 10px;
}

.changeTariff__input:focus {
  outline: none !important;
}

.calendar__changeTariff, .calendar__changeIndications {
  position: relative;
}

.calendar__changeTariff::before, .calendar__changeIndications::before {
  position: absolute;
  top: 3px;
  left: 32px;
  font-family: 'Roboto';
  font-style: normal;
  font-weight: 400;
  font-size: 12px;
  line-height: 16px;
  color: #CAC4D0;
}

.calendar__changeIndications::before {
  content: "Показания счётчика в м³ на выбранный месяц";
}

.calendar__changeTariff::before {
  content: "Цена за 1м³ воды за выбранный месяц (в рублях)";
}

</style>