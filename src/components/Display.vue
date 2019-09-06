<template>
  <div class="container">
    <button v-for="area in areas" :key="area.id" @click="changeData(area)">{{area.name}}</button>
    <blood-data ref="chart" :chartData="chartdata" :options="options"></blood-data>
    <div v-show="!loaded" class="backdrop" />
    <circle8 v-show="!loaded" class="overlay-spinner"></circle8>
  </div>
</template>
<script>
import BloodData from './BloodData'
import { Circle8 } from 'vue-loading-spinner'
export default {
  components: { BloodData, Circle8 },
  data: () => ({
    loaded: false,
    chartdata: {},
    options: {
      scales: {
        xAxes: [{
          type: 'linear',
          position: 'bottom'
        }]
      },
      responsive: true,
      maintainAspectRatio: false
    },
    areas: [],
    spec: null
  }),
  async mounted () {
    /* eslint-disable */
    this.getArea().then((areas) => {
      this.loaded = true
      this.areas = areas.map((area) => {
        return {
          name: area.get('name'),
          range: area.get('frequencies'),
          id: area.get('id')
        }
      })
    })
    /* eslint-disable */
    this.chartdata = {
      labels: [-0.6, 0.2],
      datasets: [
        {
          label: 'Demo',
          borderWidth: 1,
          pointRadius: 0,
          data: [{ x: -0.5999923808, y: 2432.0826306537833 }, { x: -0.599763805, y: 2265.323385061954 }, { x: -0.5995352292, y: 2163.1145665703098 }]
        }
      ]
    }
  },
  methods: {
    getArea () {
      return Parse.Cloud.run('getArea', {})
    },
    getSpec: async function (areaName) {
      return Parse.Cloud.run('getSpec', { area: areaName }).then(response => {
        return response;
      }).catch(err => {
        throw err
      })
    },
    changeData: async function (area) {
      this.loaded = false
      this.chartdata.datasets[0].data = await this.getSpec(area.name)
      console.log(this.$refs.chart);
      this.chartdata.labels = area.range
      this.chartdata.datasets[0].label = area.name
      this.$refs.chart.renderChart(this.chartdata, this.options)
      setTimeout(() => {
        this.loaded = true
      }, 0);
    }
  }
}
</script>
<style scoped>
.overlay-spinner {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.backdrop {
  position: absolute;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  background: black;
  opacity: 0.5;
}
</style>
