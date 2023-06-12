<script setup>
import ResidentsList from "@/components/UI/ResidentsList.vue";
import Calendar from "@/components/UI/Calendar/Calendar.vue";
import Modal from "@/components/UI/Modal/Modal.vue";
</script>
<template>
  <div class="wrapper homePage">
    <ResidentsList
        :residents="showResidents"
        @openModal="openModal"
        :Indications="Indications"
        :revenue="revenue"
        :tariff="tariff"
        :waterSpent="waterSpent"
    />
    <Calendar @changeSelectedDate="тзь кшт вум" @changeWaterSpent="changeWaterSpent"/>
    <Modal @closeModal="closeModal" :class="{'showModal':showModal}" @addResident="addResident"/>
  </div>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return {
      showModal: false,
      showResidents: [{
        fio: "Подождите идёт загрузка",
        area: "111",
        start_date: "2000-01-01"
      },],
      allResidents: [{
        fio: "Подождите идёт загрузка",
        area: "111",
        start_date: "2000-01-01"
      },],
      selectedDate: new Date('2000-01-01'),
      Indications: 0,
      revenue: 0,
      tariff: 0,
      waterSpent: 0,
    }
  },
  methods: {
    closeModal() {
      this.showModal = false
    },
    openModal() {
      this.showModal = true
    },
    addResident(fio, area, start_date) {
      const resident = {fio: fio, area: area, start_date: start_date}
      this.closeModal()
      this.allResidents.push(resident)
      this.postResident(resident)
    },
    changeSelectedDate(year, month) {
      this.selectedDate = new Date(year, month + 1)
      this.showResidents = this.allResidents.filter((resident) => {
        const residentRegDate = new Date(resident.start_date)
        return residentRegDate.getTime() <= this.selectedDate.getTime()
      })

    },
    changeWaterSpent(waterSpent, tariff) {
      if (waterSpent < 0) {
        //alert('Пожалуйста перезагрузите страницу, скорее всего вы вышли за пределы созданных дат, или неправильно внесли показания.\n\n Если же вы уверенны, что всё сделали правильно, просто продолжайте работу')
      } else {
        console.log()
        this.tariff = tariff
        this.waterSpent = waterSpent
        this.revenue = parseInt(waterSpent) * parseInt(tariff)
      }
    },
    async fetchResidents() {
      try {
        const response = await axios.get('http://127.0.0.1:8000/api/residents')
        this.allResidents = response.data
        this.showResidents = response.data
      } catch (err) {
        alert('Ошибка при загрузке пользователей')
      }
    },
    async postResident(resident) {
      try {
        const response = await axios.post('http://127.0.0.1:8000/api/residents/create', resident)
      } catch (err) {
        alert('Ошибка при добавлении пользователя')
      }
    },
  },
  mounted() {
    this.fetchResidents()
  }
}
</script>

<style>
.wrapper {
  max-width: 1620px;
  display: block;
  margin: 0 auto;
  padding: 40px;
}

.homePage {
  display: flex;
  justify-content: space-between;
}

.showModal {
  display: flex !important;
}
</style>
