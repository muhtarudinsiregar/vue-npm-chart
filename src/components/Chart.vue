<script lang="">
import { Line, mixins } from 'vue-chartjs'
const { reactiveProp } = mixins

export default Line.extend({
  mixins: [reactiveProp],
  name: 'LineChart',
  props: ['chartData', 'options', 'labels'],
  mounted () {
    // Overwriting base render method with actual data.
    this.renderChart({
      labels: [this.labels],
      datasets: [
        {
          label: this.label,
          backgroundColor: '#f87979',
          data: [40, 20, 12, 39, 10, 40, 39, 50, 40, 20, 12, 11]
        }
      ]
    })
  },
  watch: {
    data: function () {
      // this._chart.update()
      this.renderChart(this.chartData, this.options)
    }
  }
})
</script>
