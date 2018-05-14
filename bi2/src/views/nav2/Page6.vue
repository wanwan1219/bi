<template>
    <div>
        <div class="row">
            <div class="col-lg-6 col-xs-12 col-sm-6" ref="chartLine" style="height: 300px"></div>
            <div class="col-lg-6 col-xs-12 col-sm-6" ref="chartDoubleLine" style="height: 300px"></div>
        </div>
        <div class="row">
            <div class="col-lg-6 col-sm-6 col-xs-12" ref="chartPie" style="height: 300px"></div>
            <div class="col-lg-6 col-sm-6 col-xs-12" ref="chartGauge" style="height: 300px"></div>

        </div>
    </div>
</template>

<script>
import echarts from 'echarts'
import 'echarts/theme/macarons.js'

export default {

  data () {
    return {
        lineData:[
            { year: '1991', value: 3000 },
            { year: '1992', value: 4000 },
            { year: '1993', value: 3500 },
            { year: '1994', value: 5000 },
            { year: '1995', value: 4900 },
            { year: '1996', value: 6000 },
            { year: '1997', value: 7000 },
            { year: '1998', value: 9000 },
            { year: '1999', value: 1000 }
        ],
        doubleLineDate: [
            { month: 'Jan', Tokyo: 7.0, London: 3.9 },
            { month: 'Feb', Tokyo: 6.9, London: 4.2 },
            { month: 'Mar', Tokyo: 9.5, London: 5.7 },
            { month: 'Apr', Tokyo: 14.5, London: 8.5 },
            { month: 'May', Tokyo: 18.4, London: 11.9 },
            { month: 'Jun', Tokyo: 21.5, London: 15.2 },
            { month: 'Jul', Tokyo: 25.2, London: 17.0 },
            { month: 'Aug', Tokyo: 26.5, London: 16.6 },
            { month: 'Sep', Tokyo: 23.3, London: 14.2 },
            { month: 'Oct', Tokyo: 18.3, London: 10.3 },
            { month: 'Nov', Tokyo: 13.9, London: 6.6 },
            { month: 'Dec', Tokyo: 9.6, London: 4.8 }
        ],
        pieData: [
            { item: '事例一', count: 400 },
            { item: '事例二', count: 210 },
            { item: '事例三', count: 370 },
            { item: '事例四', count: 230 },
            { item: '事例五', count: 270 }
        ],
        gaugeData: [{name: "完成率", value: 30}]
    }
  },

  mounted () {
    this.drawCharts();
    this.changeWindow();
  },
  methods: {
    drawCharts () {
        this.drawLineChart();
        this.drawDoubleLineChart(); 
        this.drawPieChart();
    },
    drawLineChart () {
        let line=this.$refs.chartLine;
        let xnum=[];
        let ynum=[];

        for(let i=0 ; i<this.lineData.length ; i++){
            xnum.push(this.lineData[i].year);
            ynum.push(this.lineData[i].value);
        }

        this.chartLine = echarts.init(line, 'macarons');
        this.chartLine.setOption ({
            title: {
                text:"每年销量概览表",
                textStyle: {
                    color : "#aaa",
                    fontStyle : 'italic'
                }
            },
            legend: {
                type: "plain"
            },
            xAxis: {
                type: 'category',
                data: xnum,
                axisTick: {
                    alignWithLabel: true
                },
            },
            yAxis: {
                type: 'value'
            },
            series: [{
                data: ynum,
                type: 'line'
            }],
            tooltip: {
                trigger: 'axis',
                axisPointer: {
                    type: 'cross',
                    label: {
                        backgroundColor: '#283b56'
                    }
                }
            },
            legend: {
                data:['year', 'value']
            },
        });
    },
    drawDoubleLineChart () {
        let doubleLine=this.$refs.chartDoubleLine;
        this.chartDoubleLine=echarts.init(doubleLine, 'macarons');
        let colors = ['#5793f3', '#d14a61', '#675bba'];
        let tokyodata=[],londondata=[],monthdata=[];

        for (let i=0; i<this.doubleLineDate.length; i++){
            tokyodata.push(this.doubleLineDate[i].Tokyo);
            londondata.push(this.doubleLineDate[i].London);
            monthdata.push(this.doubleLineDate[i].month);
        }

        this.chartDoubleLine.setOption ({
            color: colors,
            tooltip: {
                trigger: 'axis',
                axisPointer: {
                    type: 'cross'
                }
            },
            legend: {
                data:['Tokyo 降水量', 'London 降水量']
            },
            grid: {
                top: 70,
                bottom: 50
            },
            xAxis: [
                {
                    type: 'category',
                    axisTick: {
                        alignWithLabel: true
                    },
                    axisLine: {
                        onZero: false,
                        lineStyle: {
                            color: colors[1]
                        }
                    },
                    axisPointer: {
                        label: {
                            formatter: function (params) {
                                return '降水量  ' + params.value
                                    + (params.seriesData.length ? '：' + params.seriesData[0].data : '');
                            }
                        }
                    },
                    data: monthdata,
                },
                {
                    type: 'category',
                    axisTick: {
                        alignWithLabel: true
                    },
                    axisLine: {
                        onZero: false,
                        lineStyle: {
                            color: colors[0]
                        }
                    },
                    axisPointer: {
                        label: {
                            formatter: function (params) {
                                return '降水量  ' + params.value
                                    + (params.seriesData.length ? '：' + params.seriesData[0].data : '');
                            }
                        }
                    },
                    data: monthdata
                }
            ],
            yAxis: [
                {
                    type: 'value'
                }
            ],
            series: [
                {
                    name:'Tokyo 降水量',
                    type:'line',
                    xAxisIndex: 1,
                    smooth: true,
                    data: tokyodata
                },
                {
                    name:'London 降水量',
                    type:'line',
                    smooth: true,
                    data: londondata
                }
            ]
        })
    },
    drawPieChart () {
        let pie=this.$refs.chartPie;
        let itemdata=[]; 
        for(let i=0; i<this.pieData.length; i++){
            this.pieData[i].name=this.pieData[i].item;
            delete this.pieData[i].item;
            this.pieData[i].value=this.pieData[i].count;
            delete this.pieData[i].count;
            itemdata.push(this.pieData[i].name)
        }

        this.chartPie = echarts.init(pie , 'macarons');
        this.chartPie.setOption ({
            title : {
                text: '各事例占比',
                x:'center'
            },
            tooltip : {
                trigger: 'item',
                formatter: "{a} <br/>{b} : {c} ({d}%)"
            },
            //图例
            legend: {
                type: 'plain',
                orient: 'vertical',
                right: 10,
                top: 20,
                bottom: 20,
                data: itemdata,
            },
            visualMap: {
                show: false,
                min: 0,
                max: 600,

                inRange: {
                    colorLightness: [0, 1]
                }
            },
            series : [
                {
                    
                    name:'访问来源',
                    type:'pie',
                    radius : '55%',
                    center: ['50%', '50%'],
                    data:this.pieData.sort(function (a, b) { return a.value - b.value; }),
                    roseType: 'radius',
                    
                    labelLine: {
                        normal: {
                            smooth: 0.2,
                            length: 10,
                            length2: 20
                        }
                    },
                    animationType: 'scale',
                    animationEasing: 'elasticOut',
                    
                }
            ]
        })
    },
    drawGaugeChart () {
        let Gauge=this.$refs.chartGauge;
        this.chartGauge=charts.init(Gauge, "macarons");
        this.chartGauge.setOption({
            tooltip : {
                formatter: "{a} <br/>{b} : {c}%"
            },
            toolbox: {
                feature: {
                    restore: {},
                    saveAsImage: {}
                }
            },
            series: [
                {
                    name: '业务指标',
                    type: 'gauge',
                    detail: {formatter:'{value}%'},
                    data: this.gaugeData,
                }
            ]
        })
    },
    changeWindow () {
        let that= this;
        window.addEventListener("resize" , function(){
            that.chartLine.resize();
            that.chartDoubleLine.resize();
            that.pie.resize();
        })
    }
  },
  
}
</script>

<style lang="scss" scoped>
    
</style>