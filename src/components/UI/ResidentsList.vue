<template>
  <div class="residentsTable">
    <div class="residentsList__header">
      <button @click="$emit('openModal')" class="resident__label" style="width: 250px">Добавить пользователя +</button>
      <button class="resident__label">Площадь участков</button>
      <button class="resident__label">Дата регистрации</button>
      <button class="resident__label">Сумма платежа</button>
    </div>
    <div class="residentsList">
      <Resident :resident="resident" v-for="resident in residents"
                :payment="Math.ceil((resident.area/calculateArea)*revenue)"/>
    </div>
    <div class="residentsList__footer">
      <span>Общая площадь: {{ calculateArea }} м²</span>
      <span>Количество пользователей: {{ residents.length }}</span>
      <span>Выручка: {{ revenue }} рублей</span>
      <span>Расход: {{ waterSpent }}м³</span>
    </div>
  </div>
</template>

<script>
import Resident from "@/components/UI/Resident.vue";

export default {
  name: "ResidentsList",
  components: {Resident},
  props: {
    residents: {
      type: Array,
    },
    dateForFilter: {
      type: Date,
    },
    revenue: {
      type: Number
    },
    tariff: {
      type: Number,
      default: 0
    },
    waterSpent: {
      type: Number,
      default: 0,
    }
  },
  computed: {
    calculateArea() {
      if (this.residents.length > 0) {
        return this.residents.reduce((previousValue, currentValue) => {
          return {area: parseInt(previousValue.area) + parseInt(currentValue.area)}
        }).area
      } else {
        return 0
      }

    }
  },

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
  display: grid;
  grid-template-columns: 306px 200px 200px 200px;
}

.residentsList__footer {
  border-bottom-left-radius: 20px;
  border-bottom-right-radius: 20px;
  align-items: center;
  padding: 16px;
  display: flex;
  justify-content: space-between;
  padding: 0 50px 0 40px;
}

.residentsList__header, .residentsList__footer {
  width: 940px;
  height: 48px;
  background: #49454F;
}

.residentsList__header > .resident__label {
  background: #2B2930 !important;
}
</style>