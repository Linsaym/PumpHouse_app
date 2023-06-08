<template>
  <div class="modal">
    <article class="modal-container">
      <img src="./icon.svg" alt="close" class="close">
      <div class="modal__title">Добавить пользователя</div>
      <div class="modal__input-block">
        <img src="" alt="" class="input-icon">
        <input type="text" class="modal__input" placeholder="ФИО">
      </div>
      <div class="modal__input-block">
        <img src="" alt="" class="input-icon">
        <input type="text" class="modal__input" placeholder="Площадь участка">
      </div>
      <div class="modal__input-block">
        <img src="" alt="" class="input-icon">
        <input type="text" class="modal__input" placeholder="Дата подключения">
      </div>
      <button class="modal__btn">Добавить</button>
    </article>
  </div>
</template>
<script>

</script>
<style scoped>

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
  z-index: 1000;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0.74);
}

.modal-container {
  position: relative;
  width: 527px;
  height: 624px;
  background: #1D1B20;
  border-radius: 31px;
  margin-left: auto;
  margin-right: auto;
  overflow: hidden;
  display: flex;
  flex-direction: column;
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
  width: 294px;
  height: 56px;
  border-radius: 4px
}

.modal__btn {
  background: #9568FF;
  border: 1px solid #4F4F4F;
  border-radius: 32px;
  font-weight: 400;
  font-size: 32px;
}

</style>