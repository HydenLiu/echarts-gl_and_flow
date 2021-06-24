<template>
	<div :ref="myRef" style="width: 100%;height:450px;"></div>
</template>

<script>
import echarts from "echarts/lib/echarts"   //echarts
import "echarts/map/js/china.js"; // 中国地图
import "echarts-gl" //3D地图插件

export default {
	name: 'Lines',
	props: {
		// 流入还是流出
		type: {
			type: String,
			default: 'flowInto' //  'flowInto' or 'outflow'
		},
		// ref一样的话可能会重复初始化echarts，导致加载不出
		myRef:{
			type: String,
			default: 'lines'
		}
	},
	data() {
		return {
			myChart: null,
			option: null,
			points: [
				{ value: [118.8062, 31.9208], itemStyle: { color: '#4ab2e5' } }
				, { value: [127.9688, 45.368], itemStyle: { color: '#4fb6d2' } }
				, { value: [110.3467, 41.4899], itemStyle: { color: '#52b9c7' } }
				, { value: [125.8154, 44.2584], itemStyle: { color: '#5abead' } }
				, { value: [116.4551, 40.2539], itemStyle: { color: '#f34e2b' } }
				, { value: [123.1238, 42.1216], itemStyle: { color: '#f56321' } }
				, { value: [114.4995, 38.1006], itemStyle: { color: '#f56f1c' } }
				, { value: [117.4219, 39.4189], itemStyle: { color: '#f58414' } }
				, { value: [112.3352, 37.9413], itemStyle: { color: '#f58f0e' } }
				, { value: [109.1162, 34.2004], itemStyle: { color: '#f5a305' } }
				, { value: [103.5901, 36.3043], itemStyle: { color: '#e7ab0b' } }
				, { value: [106.3586, 38.1775], itemStyle: { color: '#dfae10' } }
				, { value: [101.4038, 36.8207], itemStyle: { color: '#d5b314' } }
				, { value: [103.9526, 30.7617], itemStyle: { color: '#c1bb1f' } }
				, { value: [108.384366, 30.439702], itemStyle: { color: '#b9be23' } }
				, { value: [113.0823, 28.2568], itemStyle: { color: '#a6c62c' } }
				, { value: [102.9199, 25.46639], itemStyle: { color: '#96cc34' } }
				, { value: [119.4543, 25.9222], itemStyle: { color: '#a1d740' } }
			],
			site: [119.922796, 28.467630]
		}
	},
	mounted() {
		this.init()
	},
	computed: {
		linesData() {
			// 流入
			if (this.type === 'flowInto') {
				return this.points.map(item => {
					return {
						coords: [item.value, this.site],
						lineStyle: item.itemStyle
					}
				})
			} else if (this.type === 'outflow') {
				// 流出
				return this.points.map(item => {
					return {
						coords: [this.site, item.value],
						lineStyle: item.itemStyle
					}
				})
			} else {
				return []
			}
		}
	},
	methods: {
		init() {
			this.options = {
				backgroundColor: '#013954',

				tooltip: {
					trigger: 'item',
					formatter: function ( params ) {
						return params.name;
					}
				},

				geo: {
					map: 'china',
					aspectScale: 0.75, //长宽比
					zoom: 1.1,
					roam: false,
					itemStyle: {
						normal: {
							areaColor: {
								type: 'radial',
								x: 0.5,
								y: 0.5,
								r: 0.8,
								colorStops: [{
									offset: 0,
									color: '#09132c' // 0% 处的颜色
								}, {
									offset: 1,
									color: '#274d68'  // 100% 处的颜色
								}],
								globalCoord: true // 缺省为 false
							},
							shadowColor: 'rgb(58,115,192)',
							shadowOffsetX: 10,
							shadowOffsetY: 11
						},
						emphasis: {
							areaColor: '#2AB8FF',
							borderWidth: 0,
							color: 'green',
							label: {
								show: false
							}
						}
					},
					regions: [{
						name: '南海诸岛',
						itemStyle: {
							areaColor: 'rgba(0, 10, 52, 1)',

							borderColor: 'rgba(0, 10, 52, 1)',
							normal: {
								opacity: 0,
								label: {
									show: false,
									color: "#009cc9",
								}
							}
						},


					}],
				},
				series: [{
					type: 'map',
					roam: false,
					label: {
						normal: {
							show: true,
							textStyle: {
								color: '#4880FF'
							}
						},
						emphasis: {
							textStyle: {
								color: 'rgb(183,185,14)'
							}
						}
					},

					itemStyle: {
						normal: {
							borderColor: 'rgb(147, 235, 248)',
							borderWidth: 1,
							areaColor: {
								type: 'radial',
								x: 0.5,
								y: 0.5,
								r: 0.8,
								colorStops: [{
									offset: 0,
									color: '#09132c' // 0% 处的颜色
								}, {
									offset: 1,
									color: '#274d68'  // 100% 处的颜色
								}],
								globalCoord: true // 缺省为 false
							},
						},
						emphasis: {
							areaColor: 'rgb(46,229,206)',
							// shadowColor: 'rgb(12,25,50)',
							borderWidth: 0.1
						}
					},
					zoom: 1.1,
					map: 'china' //使用
				}, {
					type: 'effectScatter',
					coordinateSystem: 'geo',
					showEffectOn: 'render',
					zlevel: 1,
					rippleEffect: {
						period: 15,
						scale: 4,
						brushType: 'fill'
					},
					hoverAnimation: true,
					label: {
						normal: {
							formatter: '{b}',
							position: 'right',
							offset: [14, 0],
							color: '#1DE9B6',
							show: true
						},
					},
					itemStyle: {
						normal: {
							color: '#1DE9B6',
							// color: function (value){ //随机颜色
							// 	return "#"+("00000"+((Math.random()*16777215+0.5)>>0).toString(16)).slice(-6);
							// },
							shadowBlur: 10,
							shadowColor: '#333'
						}
					},
					symbolSize: 12,
					data: [this.points, { value: this.site }]
				}, //地图线的动画效果
					{
						type: 'lines',
						zlevel: 2,
						effect: {
							show: true,
							period: 4, //箭头指向速度，值越小速度越快
							trailLength: 0.4, //特效尾迹长度[0,1]值越大，尾迹越长重
							symbol: 'arrow', //箭头图标
							symbolSize: 7, //图标大小
						},
						lineStyle: {
							normal: {
								color: '#1DE9B6',
								width: 1, //线条宽度
								opacity: 0.1, //尾迹线条透明度
								curveness: .3 //尾迹线条曲直度
							}
						},
						data: this.linesData
					}

				]
			}
			this.myChart = echarts.init(this.$refs[this.myRef]);
			this.myChart.setOption(this.options);
			//echarts自适应

			window.addEventListener("resize",()=>{
				this.myChart.resize();
			});
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
<style lang="scss">
</style>
