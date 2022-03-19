<template>
  <div class="workers-list">
    Docelowa lista pracowników
    <table>
      <tbody>
        <tr
          v-for="worker in workersList"
          :key="worker.id"
          v-bind:style="[
            worker._age == minAge
              ? { background: '#0F0' }
              : worker._age == maxAge
              ? { background: '#FF0' }
              : { background: '#FFF' },
          ]"
        >
          <td>{{ worker._firstName }} {{ worker._lastName }}</td>
          <td>{{ worker._age }}</td>
          <td>{{ worker._salary }}</td>
        </tr>
      </tbody>
    </table>
    Średnia pensja: {{ averageSalary }} <br />
    Maksymalny wiek: {{ maxAge }}<br />
    Minimalny wiek: {{ minAge }}
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
      averageSalary: 0,
      minAge: 0,
      maxAge: 0,
    };
  }
  mounted() {
    this.averageSalary = this.getAverageSalary();
    this.minAge = this.getMinAge();
    this.maxAge = this.getMaxAge();
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
td {
  padding-right: 0.5rem;
}
</style>
