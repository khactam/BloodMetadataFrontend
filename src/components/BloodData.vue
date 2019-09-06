<script>
import { Line, mixins } from 'vue-chartjs'
export default {
  extends: Line,
  mixins: [mixins.reactiveProp],
  props: ['chartData', 'options'],
  mounted () {
    this.renderChart(this.chartData, this.options)
  },
  watch: {
    chartData: function (newValue) {
      console.log(newValue)
      this.$data._chart.update()
      this.renderChart(newValue, this.options)
    }
  }
}
</script>
<style scoped>
</style>
