<template>
  <div class="modal" v-if="modal_type">
    <article class="modal-container" v-if="modal_type=='add'">
      <img @click="closeModal" src="./icon.svg" alt="close" class="close">
      <div class="modal__title">Добавить пользователя</div>
      <div class="modal__input-block">
        <input type="text" class="modal__input" placeholder="ФИО" v-model="fio">
        <span v-if="fioValidation" class="validation">{{ fioValidation }}</span>
      </div>
      <div class="modal__input-block">
        <input type="number" min="0" step="1" class="modal__input" placeholder="Площадь участка" v-model="area">
        <span v-if="areaValidation" class="validation">{{ areaValidation }}</span>

      </div>
      <div class="modal__input-block">
        <input type="date" class="modal__input" placeholder="Дата подключения" v-model="start_date">
        <span v-if="dateValidation" class="validation">{{ dateValidation }}</span>

      </div>
      <button @click="addResident" class="modal__btn">Добавить</button>
    </article>

    <article class="modal-container" v-if="modal_type=='update'">
      <img @click="closeModal" src="./icon.svg" alt="close" class="close">
      <div class="modal__title">Редактирование и удаление пользователя</div>
      <div class="modal__input-block">
        <input type="text" class="modal__input" placeholder="ФИО" v-model="fio">
        <span v-if="fioValidation" class="validation">{{ fioValidation }}</span>
      </div>
      <div class="modal__input-block">
        <input type="number" min="0" step="1" class="modal__input" placeholder="Площадь участка" v-model="area">
        <span v-if="areaValidation" class="validation">{{ areaValidation }}</span>

      </div>
      <div class="modal__input-block">
        <input type="date" class="modal__input" placeholder="Дата подключения" v-model="start_date">
        <span v-if="dateValidation" class="validation">{{ dateValidation }}</span>

      </div>
      <div style="display: flex;justify-content:space-between; width: 370px;">
        <button @click="deleteResident" class="modal__btn-mini">Удалить</button>
        <button @click="updateResident" class="modal__btn-mini greenBG">Обновить данные</button>
      </div>
    </article>
  </div>
</template>
<script>
export default {
  name: 'modal',
  props: {
    modal_type: String | Boolean,
    resident: {
      type: Object,
    },
  },
  data() {
    return {
      fio: '',
      area: '',
      start_date: '',
      fioValidation: '',
      areaValidation: '',
      dateValidation: '',

    }
  },
  methods: {
    deleteResident() {
      this.$emit('deleteResident', this.resident.id)
    },
    updateResident() {
      if (!(this.fioValidation || this.areaValidation || this.dateValidation)) {
        this.$emit('updateResident', this.resident.id, this.fio, this.area, this.start_date)
      }
    },
    closeModal() {
      this.fioValidation = ''
      this.areaValidation = ''
      this.dateValidation = ''
      this.$emit('closeModal')
    },
    addResident() {
      this.validation()
      if (!(this.fioValidation || this.areaValidation || this.dateValidation)) {
        this.$emit('addResident', this.fio, this.area, this.start_date)
      }
    },
    validation() {
      if (this.fio.split(" ").length !== 3) {
        this.fioValidation = 'ФИО должно состоять из трёх слов \n Например:"Ивнов Иван Иванович"'
      } else {
        this.fioValidation = ''
      }
      console.log()
      if (!(this.area) || (this.area < 0) || !(Number.isInteger(this.area))) {
        this.areaValidation = 'Площадь участка не должна содержать отрицательные и дробные значения.'
      } else {
        this.areaValidation = ''
      }
      if (!this.start_date) {
        this.dateValidation = 'Пожалуйста введите дату'
      } else {
        this.dateValidation = ''
      }
    },
  },
  watch: {
    resident: {
      handler() {
        if (this.resident != null) {
          this.fio = this.resident.fio
          this.area = this.resident.area
          this.start_date = this.resident.start_date
        }
      }
    }
  }
}
</script>
<style scoped>
.modal__btn-mini {
  margin-bottom: 40px;
  width: 165px;
  height: 68px;
  background: #da2d29;
  color: #fff;
  border: 1px solid #4F4F4F;
  border-radius: 32px;
  font-weight: 400;
  font-size: 20px;
}

.greenBG {
  background: #38B87C;
}

.validation {
  margin-top: 10px;
  display: block;
  width: 345px;
  color: #900020;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

*::-webkit-scrollbar {
  background-color: transparent;
  width: 12px;
}

*::-webkit-scrollbar-thumb {
  border-radius: 99px;
  background-color: #ddd;
  border: 4px solid #fff;
}

.modal {
  display: flex;
  z-index: 1000;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0.74);
}

.modal-container {
  padding-top: 20px;
  position: relative;
  width: 527px;
  min-height: 445px;
  background: #1D1B20;
  border-radius: 31px;
  margin-left: auto;
  margin-right: auto;
  overflow: hidden;
  display: flex;
  align-items: center;
  gap: 30px;
  flex-direction: column;
}

.modal__title {
  font-size: 20px;
  width: 300px;
  text-align: center;
}

.close {
  cursor: pointer;
  position: absolute;
  width: 20px;
  height: 20px;
  right: 25px;
  top: 16px;
}

.modal__input-block {
  position: relative;
}

.modal__input {
  padding: 0 20px;
  background: #36343B;
  width: 345px;
  height: 56px;
  border-radius: 4px;
  font-weight: 400;
  font-size: 16px;
  color: #ffffff;
}

.modal__input {
  border: none;
  outline: none;
}

.modal__btn {
  margin-bottom: 40px;
  width: 345px;
  height: 68px;
  background: #9568FF;
  border: 1px solid #4F4F4F;
  border-radius: 32px;
  font-weight: 400;
  font-size: 32px;
}

</style>