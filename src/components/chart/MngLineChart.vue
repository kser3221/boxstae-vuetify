<script>
  //Importing Line class from the vue-chartjs wrapper
  import {Line} from 'vue-chartjs'
  //Exporting this so it can be used in other components
  export default { 
    extends: Line,
	props: [
		'chartData',
	],
	data: () => ({
		options: {
			legend: {
				display: false
			}
		},
	}),
    mounted () {
      //renderChart function renders the chart with the datacollection and options object.
      this.renderChart(this.chartData, this.options)
    },
	watch: {
		chartData: {
			deep: true,
			handler(val) {
				this._chart.destroy()
				this.renderChart(this.chartData, this.options)
			}
		}
	}
  }
</script>
