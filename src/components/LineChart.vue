<script>
import { Line, mixins } from 'vue-chartjs';
const { reactiveProp } = mixins;

export default {
	name: 'LineChart',
	extends: Line,
	mixins: [reactiveProp],
	props: {
		options: Object
	},
	mounted() {
		this.renderChart(this.chartData, this.options);
	}
}
</script>
