<script setup>
import ResidentsList from "@/components/UI/ResidentsList.vue";
import Calendar from "@/components/UI/Calendar/Calendar.vue";
import Modal from "@/components/UI/Modal/Modal.vue";
</script>
<template>
  <div class="wrapper homePage">
    <ResidentsList
        :residents="showResidents"
        :Indications="Indications"
        :revenue="revenue"
        :tariff="tariff"
        :waterSpent="waterSpent"
        @openModalUpdate="openModalUpdate"
        @openModalAdd="openModalAdd"
    />
    <Calendar @changeSelectedDate="changeSelectedDate" @changeWaterSpent="changeWaterSpent"/>
    <Modal :modal_type="modal_type"
           :resident="updateResident"
           @addResident="addResident"
           @closeModal="closeModal"
           @deleteResident="deleteResident"
           @updateResident="changeUpdateResident"
    />
  </div>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return {
      modal_type: false,
      updateResident: null,
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
      selectedDate: new Date('2023-01-01'),
      Indications: 0,
      revenue: 0,
      tariff: 0,
      waterSpent: 0,
    }
  },
  methods: {
    deleteResident(id) {
      this.allResidents = this.allResidents.filter(el => el.id != id)
      this.closeModal()
      axios.delete(`http://127.0.0.1:8000/api/residents/delete/${id}`)
    },
    changeUpdateResident(id, fio, area, start_date) {
      const newResident = {
        id: id,
        fio: fio,
        area: area,
        start_date: start_date
      }
      this.allResidents = this.allResidents.map(el => el.id == id ? newResident : el)
      this.closeModal()
      axios.patch(`http://127.0.0.1:8000/api/residents/update/${id}`, newResident)
    },
    openModalUpdate(resident) {
      this.modal_type = 'update'
      this.updateResident = resident
    },
    closeModal() {
      this.modal_type = false
    },
    openModalAdd() {
      this.modal_type = 'add'
    },
    addResident(fio, area, start_date) {
      const resident = {fio: fio, area: area, start_date: start_date}
      this.closeModal()
      this.allResidents.push(resident)
      this.postResident(resident)
    },
    filterResidents(residents) {
      this.showResidents = residents.filter((resident) => {
        const residentRegDate = new Date(resident.start_date)
        return residentRegDate.getTime() <= this.selectedDate.getTime()
      })
    },
    changeSelectedDate(year, month) {
      //Единица прибавляется для того, чтобы отображались пользователи зарегистрированные в этом месяце
      this.selectedDate = new Date(year, month + 1)
      this.filterResidents(this.allResidents)
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
  watch: {
    allResidents: {
      handler(newValue) {
        this.filterResidents(newValue)
        console.log('update!')
      },
      deep: true
    }
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
</style>
