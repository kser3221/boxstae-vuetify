<template>
	<div class="boxstate-traffic">
		<div class="small" v-for="(chart, i) in charts" :key="i">
			<v-row>
				<v-col cols="7">
					<v-icon>mdi-chevron-double-right</v-icon>
					<span> {{ chart.title }} : {{ chart.ip }}{{ chart.if_name }}{{ chart.sub_device_name }}</span>
				</v-col>
				<v-col cols="5">
					<v-select
					  :items="times"
					  v-model="chart.sel_time"
					  outlined
					  dense
					  @change="fillData(chart)"
					></v-select>
				</v-col>
			</v-row>
			<div>
				<span>평균(Max) : </span>
				<label>{{ chart.up_avg }}{{ chart.up_avg_unit }} ({{ chart.up_max }}{{ chart.up_max_unit }}) / {{ chart.down_avg }}{{ chart.down_avg_unit }} ({{ chart.down_max }}{{ chart.down_max_unit }})</label>
			</div>
			<mng-line-chart :chart-data="chart.data"/>
		</div>
	</div>
</template>

<script>
import MngLineChart from '@/components/chart/MngLineChart'
import dayjs from 'dayjs'

export default {
	name: 'BoxsateTraffic',
	components: {
		MngLineChart
	},
	data: () => ({
		charts: [
		{
			title: 'ETH0',
			ip: Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)),
			if_name: 'WAN0',
			sub_device_name: '',
			up_avg: 1.04,
			up_avg_unit: 'Kbps',
			up_max: 2.04,
			up_max_unit: 'Kbps',
			down_avg: 1.04,
			down_avg_unit: 'Kbps',
			down_max: 2.04,
			down_max_unit: 'Kbps',
			sel_time:1,
			data:null
		},
		{
			title: 'ETH1',
			ip: Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)),
			if_name: 'WAN0',
			sub_device_name: '',
			up_avg: 1.04,
			up_avg_unit: 'Kbps',
			up_max: 2.04,
			up_max_unit: 'Kbps',
			down_avg: 1.04,
			down_avg_unit: 'Kbps',
			down_max: 2.04,
			down_max_unit: 'Kbps',
			sel_time:1,
			data:null
		},
		{
			title: 'ETH2',
			ip: Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)),
			if_name: 'WAN0',
			sub_device_name: '',
			up_avg: 1.04,
			up_avg_unit: 'Kbps',
			up_max: 2.04,
			up_max_unit: 'Kbps',
			down_avg: 1.04,
			down_avg_unit: 'Kbps',
			down_max: 2.04,
			down_max_unit: 'Kbps',
			sel_time:1,
			data:null
		},
		{
			title: 'ETH3',
			ip: Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)),
			if_name: 'WAN0',
			sub_device_name: '',
			up_avg: 1.04,
			up_avg_unit: 'Kbps',
			up_max: 2.04,
			up_max_unit: 'Kbps',
			down_avg: 1.04,
			down_avg_unit: 'Kbps',
			down_max: 2.04,
			down_max_unit: 'Kbps',
			sel_time:1,
			data:null
		},
		{
			title: 'ETH4',
			ip: Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)),
			if_name: 'WAN0',
			sub_device_name: '',
			up_avg: 1.04,
			up_avg_unit: 'Kbps',
			up_max: 2.04,
			up_max_unit: 'Kbps',
			down_avg: 1.04,
			down_avg_unit: 'Kbps',
			down_max: 2.04,
			down_max_unit: 'Kbps',
			sel_time:1,
			data:null
		},
		{
			title: 'ETH5',
			ip: Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)),
			if_name: 'WAN0',
			sub_device_name: '',
			up_avg: 1.04,
			up_avg_unit: 'Kbps',
			up_max: 2.04,
			up_max_unit: 'Kbps',
			down_avg: 1.04,
			down_avg_unit: 'Kbps',
			down_max: 2.04,
			down_max_unit: 'Kbps',
			sel_time:1,
			data:null
		},
		{
			title: 'ETH6',
			ip: Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)),
			if_name: 'WAN0',
			sub_device_name: '',
			up_avg: 1.04,
			up_avg_unit: 'Kbps',
			up_max: 2.04,
			up_max_unit: 'Kbps',
			down_avg: 1.04,
			down_avg_unit: 'Kbps',
			down_max: 2.04,
			down_max_unit: 'Kbps',
			sel_time:1,
			data:null
		},
		{
			title: 'ETH',
			ip: Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)),
			if_name: 'WAN0',
			sub_device_name: '',
			up_avg: 1.04,
			up_avg_unit: 'Kbps',
			up_max: 2.04,
			up_max_unit: 'Kbps',
			down_avg: 1.04,
			down_avg_unit: 'Kbps',
			down_max: 2.04,
			down_max_unit: 'Kbps',
			sel_time:1,
			data:null
		},
		{
			title: 'LAN0',
			ip: Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)) + '.' + Math.floor((Math.random()*255)),
			if_name: 'WAN0',
			sub_device_name: '',
			up_avg: 1.04,
			up_avg_unit: 'Kbps',
			up_max: 2.04,
			up_max_unit: 'Kbps',
			down_avg: 1.04,
			down_avg_unit: 'Kbps',
			down_max: 2.04,
			down_max_unit: 'Kbps',
			sel_time:1,
			data:null
		},
		],
		times: [
			{ value: 1, text: '6h'},
			{ value: 2, text: '12h'},
			{ value: 3, text: '24h'},
			{ value: 4, text: '1Week'},
			{ value: 5, text: '1Month'},
			{ value: 6, text: '1Year'},
		],
	}),
	methods: {
		fillData (c) {
			this.charts.map((d, i) => {
				if(c && c.title) {
					if(c.title != d.title) {
						return;
					}
				}
				d.data = {
					labels: this.getTime(c.sel_time),
					datasets: [
						{
							label: 'UP',
							backgroundColor: '#f87979',
							data: [this.getRandomInt(), this.getRandomInt(), this.getRandomInt(), this.getRandomInt()]
						},
						{
							label: 'DOWN',
							backgroundColor: '#387979',
							data: [-(this.getRandomInt()), -(this.getRandomInt()), -(this.getRandomInt()), -(this.getRandomInt())]
						}
					]
				}

				d.up_avg = this.getAvg(d.data.datasets[0].data);
				d.up_max = this.getMax(d.data.datasets[0].data);
				d.down_avg = this.getAvg(d.data.datasets[1].data);
				d.down_max = this.getMax(d.data.datasets[1].data);
			});
			/*
			this.cpuData = {
				labels: [this.getRandomInt(), this.getRandomInt()],
				datasets: [
					{
						label: 'Data One',
	   					backgroundColor: '#f87979',
	   					data: [this.getRandomInt(), this.getRandomInt()]
					}
				]
			}
			*/
		},
		getRandomInt () {
			return Math.floor(Math.random() * (50 - 5 + 1)) + 5
		},
		getTime(type) {
			var range = [];
			var now = dayjs().format("YYYY-MM-DD HH:mm:ss");

			if(type == 1) {
				range.push(dayjs(now).subtract(6, 'hour').format('HH:mm'));
				range.push(dayjs(now).subtract(4.5, 'hour').format('HH:mm'));
				range.push(dayjs(now).subtract(3, 'hour').format('HH:mm'));
				range.push(dayjs(now).subtract(1.5, 'hour').format('HH:mm'));
			} else if(type == 2) {
				range.push(dayjs(now).subtract(12, 'hour').format('HH:mm'));
				range.push(dayjs(now).subtract(9, 'hour').format('HH:mm'));
				range.push(dayjs(now).subtract(6, 'hour').format('HH:mm'));
				range.push(dayjs(now).subtract(3, 'hour').format('HH:mm'));
			} else if(type == 3) {
				range.push(dayjs(now).subtract(24, 'hour').format('HH:mm'));
				range.push(dayjs(now).subtract(18, 'hour').format('HH:mm'));
				range.push(dayjs(now).subtract(12, 'hour').format('HH:mm'));
				range.push(dayjs(now).subtract(6, 'hour').format('HH:mm'));
			} else if(type == 4) {
				range.push(dayjs(now).subtract(4, 'day').format('DD일 HH시'));
				range.push(dayjs(now).subtract(3, 'day').format('DD일 HH시'));
				range.push(dayjs(now).subtract(2, 'day').format('DD일 HH시'));
				range.push(dayjs(now).subtract(1, 'day').format('DD일 HH시'));
			} else if(type == 5) {
				range.push(dayjs(now).subtract(4, 'week').format('MM/DD'));
				range.push(dayjs(now).subtract(3, 'week').format('MM/DD'));
				range.push(dayjs(now).subtract(2, 'week').format('MM/DD'));
				range.push(dayjs(now).subtract(1, 'week').format('MM/DD'));
			} else if(type == 6) {
				range.push(dayjs(now).subtract(12, 'month').format('YY년 MM월'));
				range.push(dayjs(now).subtract(9, 'month').format('YY년 MM월'));
				range.push(dayjs(now).subtract(6, 'month').format('YY년 MM월'));
				range.push(dayjs(now).subtract(3, 'month').format('YY년 MM월'));
			}
			return range;
		},
		getAvg(arr) {
			var sum = arr => arr.reduce((a,c) => (a += c),0); // add equals and init with 0
			var avg = sum(arr) / arr.length;
			return avg;
		},
		getMax(arr) {
			return Math.max.apply(Math, arr);
		},
	},
	created() {
		this.fillData({sel_time: 1})
	},
}
</script>

<style scoped>
.small {
	display: inline-block;
	width: 30%;
	margin: 10px;
}
</style>
