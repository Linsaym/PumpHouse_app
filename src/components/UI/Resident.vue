<template>
  <div class="resident">
    <div class="icon">{{ resident.fio[0] }}</div>
    <span class="fio">{{ resident.fio }}</span>
    <div class="resident__label">
      участок: {{ resident.area }} м²
    </div>
    <div class="resident__label">
      {{ resident.start_date }}
    </div>
    <div class="resident__label">
      {{ payment }}₽
    </div>
  </div>
</template>

<script>
export default {
  name: "Resident",
  props: {
    resident: {
      type: Object,
    },
    payment: {
      type: Number,
    }

  },
}
</script>

<style>
.resident {
  position: relative;
  z-index: 1;
  display: grid;
  align-items: center;
  grid-template-columns: 66px 240px 200px 200px 200px;
  width: 940px;
  height: 56px;
  background: #1D1B20;
}

.icon {
  width: 40px;
  height: 40px;
  background: #4F378B;
  border-radius: 100px;
  margin-left: 16px;
  display: flex;
  justify-content: center;
  line-height: 40px;
}

.fio {
  font-weight: 400;
  font-size: 16px;
}

.resident__label {
  border: none;
  color: #E8DEF8;
  width: 176px;
  margin: 0 20px;
  max-width: 300px;
  display: inline-block;
  height: 30px;
  background: #5A5A5A;
  border-radius: 100px;
  text-align: center;
  align-items: center;
  line-height: 30px;
}
</style>