<template>
  <div class="calendar">
    <div class="calendar__header">Редактирование тарифа за месяц</div>
    <div class="calendar__year">
      <YearSlider @changeSelectedYear="(year)=>changeSelectedDate(year,0)"/>
    </div>
    <div class="calendar__main">
      <CalendarMonth v-for="(month,i) in months"
                     :key="indicationsAndTariffs[i].indications + month + indicationsAndTariffs[i].tariff"
                     :month="month"
                     :isSelected="i==selectedDate.month"
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
import axios from "axios";

export default {
  name: "Calendar",
  components: {
    YearSlider,
    CalendarMonth
  },
  data() {
    return {
      inputType: '',
      selectedDate: {year: 2023, month: 0},
      months: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'],
      indicationsAndTariffs: IndicationsAndTariffs[2023],
      AllYears: {
        "2023": [
          {tariff: 50, indications: 34234},
          {tariff: 53, indications: 45923},
          {tariff: 50, indications: 46651},
          {tariff: 56, indications: 49452},
          {tariff: 55, indications: 56123},
          {tariff: 50, indications: 58231},
          {tariff: 59, indications: 58422},
          {tariff: 58, indications: 59231},
          {tariff: 56, indications: 60231},
          {tariff: 50, indications: 61231},
          {tariff: 53, indications: 62312},
          {tariff: 54, indications: 74231},
        ],
      },
      tariffOnSelectedMonth: 0,
      IndicationsOnSelectedMonth: 0,
    }
  },
  methods: {
    changeSelectedDate(year, month) {
      if (this.AllYears[year] == undefined) {
        this.fetchYear(year).then(() => {
          this.indicationsAndTariffs = this.AllYears[year]
          this.selectedDate.year = year
          this.selectedDate.month = month
        })
      } else {
        this.indicationsAndTariffs = this.AllYears[year]
        this.selectedDate.year = year
        this.selectedDate.month = month
      }

      //Если выбран Январь, то нам понадобятся данные о счётчике прошлого года. Поэтому всегда обязательно иметь данные о текущем и предыдущем годе
      if (this.AllYears[year - 1] == undefined) {
        this.fetchYear(year - 1)
      }
    },
    changeInputOnTariff() {
      this.inputType = 'Tariff'
    },
    changeInputOnIndications() {
      this.inputType = 'Indications'
    },
    changeSelectedMonth(selectedMonthIndex) {
      this.selectedDate.month = selectedMonthIndex
      this.tariffOnSelectedMonth = this.indicationsAndTariffs[this.selectedDate.month].tariff
      this.IndicationsOnSelectedMonth = this.indicationsAndTariffs[this.selectedDate.month].indications
    },
    changeTariff(event) {
      const newTariff = event.target.innerText.substring(0, 5)
      this.tariffOnSelectedMonth = newTariff
      this.indicationsAndTariffs[this.selectedDate.month].tariff = newTariff
      this.updateTariff(this.selectedDate.year, this.selectedDate.month, newTariff)
    },
    changeIndications(event) {
      const newIndications = event.target.innerText.substring(0, 6)
      this.IndicationsOnSelectedMonth = newIndications
      this.indicationsAndTariffs[this.selectedDate.month].indications = newIndications
      this.updateIndications(this.selectedDate.year, this.selectedDate.month, newIndications)
    },
    async fetchYear(year) {
      try {
        const response = await axios.get(`http://127.0.0.1:8000/api/get_periods/${year}`)
        let thisYear = {}
        thisYear[year] = []//Создаём пустой массив, чтобы потом в него добавлять объекты
        response.data.forEach((elem) => {
          thisYear[year][elem.month] = {tariff: elem.tariff, indications: elem.indications}
        })
        this.AllYears[year] = thisYear[year]
      } catch (err) {
        alert('Ошибка при добавлении пользователя')
      }
    },
    async updateTariff(year, month, tariff) {
      const response = await axios.put(`http://127.0.0.1:8000/api/update_indications/${year}/${month}`, {tariff: tariff})
    },
    async updateIndications(year, month, indications) {
      const response = await axios.put(`http://127.0.0.1:8000/api/update_indications/${year}/${month}`, {indications: indications})
    },
  },
  mounted() {
    this.fetchYear(2023).then(() => {
      this.indicationsAndTariffs = this.AllYears[2023]
    })
    this.fetchYear(2022)
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