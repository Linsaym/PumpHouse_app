<template>
  <div class="slider">
    <div @click="prevYear" class="arrow-left">←</div>
    <div class="years">
      <span @click="prevYear" class="prevYear">{{ year - 1 }}</span>
      <span class="year">{{ year }}</span>
      <span @click="nextYear" class="nextYear">{{ year + 1 }}</span>
    </div>
    <div @click="nextYear" class="arrow-right">→</div>
  </div>
</template>

<script>


export default {
  name: 'yearSlider',
  data() {
    return {
      year: 2023,
    }
  },
  methods: {
    prevYear() {
      this.year -= 1
      this.$emit('changeSelectedYear', this.year)
    },
    nextYear() {
      this.year += 1
      this.$emit('changeSelectedYear', this.year)
    }
  },
  mounted() {
    const today = new Date()
    this.year = today.getFullYear()
  }
};
</script>

<style scoped>
.slider {
  display: flex;
  width: 100%;
  padding: 0px 20px;
  justify-content: space-between;
}

.arrow-right, .arrow-left {
  cursor: pointer;
  font-size: 40px;
  -moz-user-select: none;
  -khtml-user-select: none;
  -webkit-user-select: none;
  user-select: none;
}

.years {
  position: relative;
  width: 200px;
}

.years > * {
  position: absolute;
}

.year {
  left: calc(50% - 23px);
}

.prevYear, .nextYear {
  cursor: pointer;
  top: 2px;
  color: #CAC4D0;
  font-size: 16px;
}

.prevYear {
  left: 10px;
}

.nextYear {
  right: 10px;
}
</style>