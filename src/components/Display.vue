<template>
  <div class="container">
    <blood-data :chartData="chartdata" :options="options"></blood-data>
  </div>
</template>
<script>
import BloodData from './BloodData'
export default {
  components: { BloodData },
  props: {
    selectedArea: {},
    spec: null
  },
  watch: {
    selectedArea: {
      handler: function (newValue) {
        this.chartdata.datasets.label = newValue.range
        this.chartdata.datasets[0].label = newValue.name
      },
      deep: true
    },
    spec: {
      handler: function (newValue) {
        this.chartdata.datasets[0].data = newValue
        console.log(this.chartdata)
      },
      deep: true
    }
  },
  data: () => ({
    loaded: false,
    chartdata: {},
    options: null
  }),
  async mounted () {
    this.options = {
      scales: {
        xAxes: [{
          type: 'linear',
          position: 'bottom'
        }]
      },
      responsive: true,
      maintainAspectRatio: false
    }
    /* eslint-disable */
    this.chartdata = {
      labels: [-0.6, 0.2],
      datasets: [
        {
          label: 'Things',
          data: [{ x: -0.5999923808, y: 2432.0826306537833 }, { x: -0.599763805, y: 2265.323385061954 }, { x: -0.5995352292, y: 2163.1145665703098 }, { x: -0.5990780777, y: 2129.988906045488 }, { x: -0.5988495019, y: 2140.93779104109 }
            , { x: -0.5986209261, y: 2133.5425598753936 }
            , { x: -0.5983923503, y: 2090.800070597172 }
            , { x: -0.5981637745, y: 2012.0625780884388 }
            , { x: -0.5979351988, y: 1919.7399740355147 }
            , { x: -0.597706623, y: 1846.537290743382 }
            , { x: -0.5974780472, y: 1810.7444314186312 }
            , { x: -0.5972494714, y: 1806.0989308146713 }
            , { x: -0.5970208956, y: 1814.2930511508116 }
            , { x: -0.5967923199, y: 1824.0300422933472 }
            , { x: -0.5965637441, y: 1835.3435160195309 }
            , { x: -0.5963351683, y: 1842.6327904205255 }
            , { x: -0.5961065925, y: 1819.6928520895342 }]
        }
      ]
    }
  },
  methods: {
  }
}
</script>
<style scoped>
</style>
