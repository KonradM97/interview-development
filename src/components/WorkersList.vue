<template>
  <div class="workers-list">
    <h2>Docelowa lista pracowników</h2>
    <div
      v-for="worker in workersList"
      :key="worker.id"
      :style="[
        worker._age == minAge
          ? { background: '#0F0' }
          : worker._age == maxAge
          ? { background: '#FF0' }
          : { background: '#FFF' },
      ]"
      class="worker"
    >
      <div class="worker__dataField worker__name">{{ worker._firstName }} {{ worker._lastName }}</div>
      <div class="worker__dataField worker__age">{{ worker._age }}</div>
      <div class="worker__dataField worker__salary">{{ worker._salary }}</div>
    </div>
    <div class="workers-list__moreInfo">
    Średnia pensja: {{ averageSalary }} <br />
    Maksymalny wiek: {{ maxAge }}<br />
    Minimalny wiek: {{ minAge }}
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import { Component, Prop } from "vue-property-decorator";

@Component({ name: "WorkersList" })
export default class WorkersList extends Vue {
  @Prop({ type: Array, required: false, default: () => [] }) workersList;
  data() {
    return {
      averageSalary: this.getAverageSalary(),
      minAge: this.getMinAge(),
      maxAge: this.getMaxAge(),
    };
  }
  /**
   *
   * @returns {number}
   */
  getAverageSalary() {
    let lenght = Object.keys(this.workersList).length;
    let sum = 0;
    for (var i = 0; i < lenght; i++) {
      sum += this.workersList[i]._salary;
    }
    if (lenght != 0) {
      return sum / lenght;
    } else {
      return 0;
    }
  }
  /**
   *
   * @returns {number}
   */
  getMinAge() {
    const ages = this.workersList.map((a) => a._age);
    return Math.min.apply(null, ages);
  }
  /**
   *
   * @returns {number}
   */
  getMaxAge() {
    const ages = this.workersList.map((a) => a._age);
    return Math.max.apply(null, ages);
  }
}
</script>

<style lang="scss" scoped>

.workers-list {
  padding: 0.5rem;
}
.worker {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: minmax(1fr, 100px);
}
.workers-list__moreInfo
{
  text-align: center;
}
</style>