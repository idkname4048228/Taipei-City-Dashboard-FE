<!-- Developed by Taipei Urban Intelligence Center 2023 -->

<script setup>
import { ref } from 'vue';

const props = defineProps(['chart_config', 'activeChart', 'series']);
const chartOptions = ref({
	chart: {
		toolbar: {
			show: false,
			tools: {
				zoom: false,
			},
		},
	},
	grid: {
		show: true,  
		row: {
			opacity: 0
		},  
	},
	legend: {
		show: props.chart_config.categories ? true : false,
	},
	markers: {
		hover: {
			size: 5,
		},
		size: 3,
		strokeWidth: 0,
	},
	stroke: {
		colors: props.chart_config.color,
		width: [0, 2],
	},
	tooltip: {
		// The class "chart-tooltip" could be edited in /assets/styles/chartStyles.css
		custom: function ({ series, seriesIndex, dataPointIndex, w }) {
			return '<div class="chart-tooltip">' +
				'<h6>' + w.globals.seriesNames[seriesIndex] + 
				` - ${w.globals.labels[dataPointIndex]}` +
				` ${props.chart_config.unit}` + '</h6>' +
				'<span>'+ series[seriesIndex * 2][dataPointIndex] + props.chart_config.leftUnit + ' </span>' +
				'<h6>' +  w.globals.seriesNames[seriesIndex + 1] + 
				` - ${w.globals.labels[dataPointIndex]}` +
				` ${props.chart_config.unit}` + '</h6>' +
				'<span>'+ series[seriesIndex * 2 + 1][dataPointIndex] + props.chart_config.rightUnit + ' </span>' +
				'</div>';
		},
		enabled: true,
		shared: true,
		followCursor: true,
	},
	// labels: props.chart_config.labels,
	xaxis: {
		crosshairs: {
			show: true,
		},
		tooltip: {
			enabled: false,
		},
		categories: props.chart_config.categories
	},
	yaxis: [{}, {opposite: true}],
});

</script>

<template>
	<div v-if="activeChart === 'MixLineColumnChart'">
		<apexchart width="100%" height="270px" type="line" :options="chartOptions" :series="series"
			@dataPointSelection="handleDataSelection"></apexchart>
	</div>
</template>