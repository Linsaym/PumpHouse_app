<template>
  <div class="calendar">
    <div class="calendar__header">Редактирование тарифа за месяц</div>
    <div class="calendar__year">
      <YearSlider @changeSelectedYear="changeSelectedYear" :selectedYear="selectedYear" :active="selectedMonth!=null"/>
    </div>
    <div class="calendar__main">
      <CalendarMonth v-for="(month,i) in months" :key="indicationsAndTariffs[i].indications + month" :month="month"
                     :isSelected="i==selectedMonth"
                     :monthIndex="i"
                     :indicationsAndTariff="indicationsAndTariffs[i]"
                     @changeSelectedMonth="changeSelectedMonth"
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
import IndicationsAndTariffs from "@/data_for_tests/Indications";

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
      indicationsAndTariffs: IndicationsAndTariffs[2023],
      AllYears: IndicationsAndTariffs,
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
      this.tariffOnSelectedMonth = this.indicationsAndTariffs[this.selectedMonth].tariff
      this.IndicationsOnSelectedMonth = this.indicationsAndTariffs[this.selectedMonth].indications
      this.changeSelectedDate()
    },
    changeTariff(event) {
      const newTariff = event.target.innerText.substring(0, 5)
      this.tariffOnSelectedMonth = newTariff
      this.indicationsAndTariffs[this.selectedMonth].tariff = newTariff
    },
    changeIndications(event) {
      const newIndications = event.target.innerText.substring(0, 6)
      this.IndicationsOnSelectedMonth = newIndications
      this.indications[this.selectedMonth] = newIndications
    },
    changeSelectedYear(year) {
      if (this.selectedMonth == null) {
        alert('Пожалуйста сначала выберите месяц')
      } else {
        this.selectedYear = year
        this.changeSelectedDate()
      }
    },
    changeSelectedDate() {
      this.indicationsAndTariffs = this.AllYears[this.selectedYear]
      //Если мы находимся на первом месяце, значит, чтобы посчитать разницу счётчиков, нужно взять текущую дату и отнять от неё показания на последнем дне предыдущего года
      let waterSpent
      try {
        waterSpent = this.selectedMonth == 0 ?
            this.IndicationsOnSelectedMonth - this.AllYears[this.selectedYear - 1][11].indications
            : this.IndicationsOnSelectedMonth - this.indicationsAndTariffs[this.selectedMonth - 1].indications
      } catch (err) {
        alert('Данные о датах ранее 2021 года ещё не занесены, это тестовая версия приложения. Перезагрузите страницу, чтобы продолжить работу')
        waterSpent = 0
      }
      this.$emit('changeSelectedDate', this.selectedYear, this.selectedMonth, waterSpent, this.tariffOnSelectedMonth)
    }
  },
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