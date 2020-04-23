<template>
	<div>
		<h2 id="area-title">{{ this.areaName }}</h2>
		<b-card title="Confirmed cases" class="chart-card" no-body>
			<b-tabs card>
				<b-tab title="Linear">
					<LineChart v-if="data" :chartData="{labels, datasets: confirmedDatasets}" :options="chartOptions('linear')" :width="isMobile() ? 1 : 16" :height="isMobile() ? 1 : 9"/>
				</b-tab>
				<b-tab title="Logarithmic">
					<LineChart v-if="data" :chartData="{labels, datasets: confirmedDatasets}" :options="chartOptions('logarithmic')" :width="isMobile() ? 1 : 16" :height="isMobile() ? 1 : 9"/>
				</b-tab>
			</b-tabs>
		</b-card>
		<b-card title="New confirmed cases" class="chart-card">
			<BarChart v-if="data" :chartData="{labels, datasets: newConfirmedDatasets}" :options="chartOptions('linear')" :width="isMobile() ? 1 : 16" :height="isMobile() ? 1 : 9"/>
		</b-card>
	</div>
</template>

<script>
import LineChart from './LineChart';
import BarChart from './BarChart';
import {max} from 'lodash';
import moment from 'moment';

export default {
	name: 'Dashboard',
	props: {
		data: Object,
		areaName: String
	},
	components: {
		LineChart,
		BarChart
	},
	computed: {
		labels() {
			return this.data.timestamps.map(timestamp => moment(timestamp).format('DD. MM.'));
		},
		confirmedDatasets() {
			if (this.areaName !== null) {
				const raw = this.data.areaData.find(obj => obj.name.trim() === this.areaName);
				return [{
					label: raw.name,
					backgroundColor: raw.color,
					borderColor: raw.color + "55",
					fill: false,
					data: raw.timeline.map(obj => obj.confirmed)
				}];
			}

			return this.data.areaData.map(obj => {
				return {
					label: obj.name,
					backgroundColor: obj.color,
					borderColor: obj.color + "55",
					fill: false,
					data: obj.timeline.map(obj => obj.confirmed)
				}
			}).sort((a, b) => max(b.data) - max(a.data)).slice(0, 5);
		},
		newConfirmedDatasets() {
			if (this.areaName !== null) {
				const raw = this.data.areaData.find(obj => obj.name.trim() === this.areaName);
				return [{
					label: raw.name,
					backgroundColor: raw.color,
					borderColor: raw.color + "55",
					fill: false,
					data: raw.timeline.map(obj => obj.newConfirmed)
				}];
			}

			return this.data.areaData.map(obj => {
				return {
					label: obj.name,
					backgroundColor: obj.color,
					borderColor: obj.color + "55",
					fill: false,
					data: obj.timeline.map(obj => obj.newConfirmed)
				}
			}).sort((a, b) => max(b.data) - max(a.data)).slice(0, 5);
		}
	},
	methods: {
		isMobile() {
			return /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
		},
		chartOptions(chartScaleType) {
			return {
				scales: {
					yAxes: [
						{
							type: chartScaleType
						}
					]
				},
				tooltips: {
					mode: 'index',
					intersect: false
				},
				hover: {
					mode: 'index',
					intersect: true
				},
				responsive: true,
				maintainAspectRatio: true
			};
		}
	}
};
</script>

<style scoped>
#area-title {
	margin-top: 20px;
}
.chart-card {
	margin-top: 20px;
}
</style>
