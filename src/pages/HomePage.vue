<script setup>
import ResidentsList from "@/components/UI/ResidentsList.vue";
import Calendar from "@/components/UI/Calendar/Calendar.vue";
import Modal from "@/components/UI/Modal/Modal.vue";
</script>
<template>
  <div class="wrapper homePage">
    <ResidentsList :residents="residents" @openModal="openModal" :Indications="Indications"/>
    <Calendar @changeSelectedDate="changeSelectedDate"/>
    <Modal @closeModal="closeModal" :class="{'showModal':showModal}" @addResident="addResident"/>
  </div>
</template>
<script>
import Residents from "@/data_for_tests/residents";

export default {
  data() {
    return {
      showModal: false,
      residents: Residents,
      selectedDate: new Date('2000-01-01'),
      Indications: 0
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
      this.closeModal()
      this.residents.push({fio: fio, area: area, start_date: start_date})
    },
    changeSelectedDate(year, month, waterSpent, tariff) {
      this.selectedDate = new Date(year, month + 1)
      this.residents = Residents.filter((resident) => {
        const residentRegDate = new Date(resident.start_date)
        return residentRegDate.getTime() <= this.selectedDate.getTime()
      })
      if (waterSpent <= 0) {
        alert('Пожалуйста перезагрузите страницу, скорее всего вы вышли за пределы созданных дат')
      }

    }
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
