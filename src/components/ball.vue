<template>
	<div ref="ball" style="width: 100%;height:500px;" />
</template>

<script>
import echarts from "echarts/lib/echarts";

export default {
	data() {
		return {
			myChart: null,
			options: null,
			data: [
				{
					name: '学习',
					value: '30%',
					color: '#7042E1',
				},
				{
					name: '医疗',
					value: '10%',
					color: '#4798DF',
				},
				{
					name: '店面经营',
					value: '10%',
					color: '#983224',
				},
				{
					name: '其他',
					value: '10%',
					color: '#A2892F',
				},
				{
					name: '务工',
					value: '25%',
					color: '#079A79',
				},
				{
					name: '经商',
					value: '25%',
					color: '#205DC1',
				},
			],
			offsetData: [
				[80, 53],
				[35, 73],
				[30, 33],
				[60, 33],
				[10, 48],
				[9, 48],
			],
			symbolSizeData: [200, 185, 175, 165, 135, 112],
		}
	},
	mounted() {
		this.init()
	},
	methods: {
		init() {
			let datas = this.data.map(( item, i ) => {
				return {
					name: item.name + '\n' + item.value + '人',
					value: this.offsetData[i],
					symbolSize: this.symbolSizeData[i],
					label: {
						normal: {
							textStyle: {
								fontSize: 14,
							},
						},
					},
					itemStyle: {
						normal: {
							color: new echarts.graphic.RadialGradient(0.5, 0.3, 0.7, [
								{
									offset: 0,
									color: 'rgba(255,255,255,0.6)',
								},
								{
									offset: 1,
									color: item.color,
								},
							]),
							opacity: 0.8,
							shadowColor: item.color,
							shadowBlur: 10,
							shadowOffsetX: 1,
							shadowOffsetY: 1,
						},
					},
				}
			})

			this.options = {
				backgroundColor: '#0e2147',
				grid: {
					show: false,
					top: 10,
					bottom: 10,
				},
				xAxis: [
					{
						gridIndex: 0,
						type: 'value',
						show: false,
						min: 0,
						max: 100,
						nameLocation: 'middle',
						nameGap: 5,
					},
				],
				yAxis: [
					{
						gridIndex: 0,
						min: 0,
						show: false,
						max: 100,
						nameLocation: 'middle',
						nameGap: 30,
					},
				],

				series: [
					{
						type: 'scatter',
						symbol: 'circle',
						symbolSize: 120,
						label: {
							normal: {
								show: true,
								formatter: '{b}',
								color: '#fff',
								textStyle: {
									fontSize: '20',
								},
							},
						},

						itemStyle: {
							normal: {
								color: '#00acea',
							},
						},
						data: datas,
					},
				],
			};

			this.myChart = echarts.init(this.$refs.ball);
			this.myChart.setOption(this.options);
			window.onresize = this.myChart.resize;
		}
	},
	beforeDestroy() {
		if (!this.myChart) {
			return;
		}
		this.myChart.dispose();
		this.myChart = null;
	},
}
</script>

<style lang="scss" scoped>

</style>
