<template>
  <div class="residentsTable">
    <div class="residentsList__header">
      <button class="resident__label" style="width: 250px">Добавить пользователя +</button>
      <button class="resident__label">Площадь участков</button>
      <button class="resident__label">Дата регистрации</button>
    </div>
    <div class="residentsList">
      <Resident :resident="resident" v-for="resident in residents"/>
    </div>
    <div class="residentsList__footer">
      <span>Общая площадь: {{ areaSum }} м²</span>
      <span>Выручка: {{ revenue }} рублей</span>
    </div>
  </div>
</template>

<script>
import Resident from "@/components/UI/Resident.vue";

export default {
  name: "ResidentsList",
  components: {Resident},
  data() {
    return {
      tariff: 98,
      areaSum: 0,
      revenue: 0,
      residents: [
        {
          fio: "Иванов иван Иванович",
          area: "112",
          start_date: "2022-10-01"
        },
        {
          fio: "Ибрагим Генадий Львович",
          area: "52",
          start_date: "2023-02-01"
        },
        {
          fio: "Загвоздкин Вячеслав Юрьевич",
          area: "120",
          start_date: "2018-01-04"
        },
        {
          fio: "Петр III Великий",
          area: "2481",
          start_date: "2023-02-01"
        },
        {
          fio: "Грекова Полина Вадимовна",
          area: "321",
          start_date: "2023-02-01"
        },
        {
          fio: "Савельев Сергей Артёмович",
          area: "68",
          start_date: "2023-02-01"
        },
        {
          fio: "Черкасова Асия Тимофеевна",
          area: "421",
          start_date: "2023-02-01"
        },
        {
          fio: "Завьялова Валерия Платоновна",
          area: "18",
          start_date: "2023-02-01"
        },
        {
          fio: "Ибрагим Главный",
          area: "520",
          start_date: "2008-02-01"
        },
        {
          fio: "Степанов Всеволод Александрович",
          area: "40",
          start_date: "2008-02-01"
        },
        {
          fio: "Васильев Арсений Иванович",
          area: "31",
          start_date: "2008-02-01"
        },
        {
          fio: "Артемов Михаил Даниилович",
          area: "28",
          start_date: "2008-02-01"
        },
        {
          fio: "Титова Олеся Артёмовна",
          area: "36",
          start_date: "2008-02-01"
        },
        {
          fio: "Егоров Алексей Андреевич",
          area: "87",
          start_date: "2008-02-01"
        },
        {
          fio: "Калашников Фёдор Андреевич",
          area: "73",
          start_date: "2008-02-01"
        },

      ],
    }
  },
  mounted() {
    this.areaSum =
        this.residents.reduce((previousValue, currentValue) => {
          return {area: parseInt(previousValue.area) + parseInt(currentValue.area)}
        }).area
    this.revenue = this.tariff * this.areaSum

  }
}
</script>
<style>
.residentsTable {
  display: flex;
  flex-direction: column;
}

.residentsList {
  max-height: 680px;
  overflow-y: scroll;
}

.residentsList::-webkit-scrollbar {
  width: 0;
}

.residentsList > *:not(:last-child)::after {
  position: absolute;
  z-index: 4;
  left: 35px;
  bottom: 0px;
  content: "";
  height: 0px;
  width: 400px;
  border: 1px solid #49454F;
}

.residentsList__header {
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  align-items: center;
  grid-template-columns: 306px 200px 200px;
}

.residentsList__footer {
  border-bottom-left-radius: 20px;
  border-bottom-right-radius: 20px;
  align-items: center;
  padding: 16px;
  grid-template-columns: 360px 360px;
}

.residentsList__header, .residentsList__footer {
  width: 747px;
  height: 48px;
  background: #49454F;
  display: grid;
}

.residentsList__header > .resident__label {
  background: #2B2930 !important;
}
</style>