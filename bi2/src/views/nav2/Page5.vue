<template>
    <div>
        <div class="line-wrap">
            <div id="line-chart"></div>
        </div>
        <div class="histogram-wrap">
            <div class="histogram-top">
                <span @click="showhistogram=true" class="cu">数据1</span>
                <span @click="showhistogram=false" class="cu">数据2</span>
            </div>
            <div class="histogram-con-wrap row">
                <div class="col-lg-8 col-sm-8 col-xs-12 histogram-lt">
                    <div class="histogram-con clearfix" :class='{"showlt":showhistogram,"showrt":!showhistogram}'>
                        <div id="histogram1" class="fl"></div>
                        <div id="histogram2" class="fl"></div>
                    </div>
                </div>
                <div class="histogram-data-analysis col-lg-4 col-sm-4 col-xs-12">
                    <p class="ct">数据解析</p>
                    <div v-for="(item,index) in histogramAnalysis" :key="item">
                        <p class="ct">{{index+1}} =.= {{item}}</p>
                    </div>
                </div>
            </div>
        </div>
        <div class="content">
            <div class="row">
                <div class="pie-wrap col-lg-5 col-xs-12">
                    <div id="pie-chart"></div>
                </div>
                <div class="curve-wrap col-lg-7 col-xs-12">
                    <div id="curve-chart"></div>
                </div>
                
            </div>
        </div> 
        <div class="sliced-wrap">
            <div class="slide-classify">
                <span @click="sliced1">图一 </span>
                <span @click="sliced2"> 图二 </span>
                <span @click="sliced3"> 图三</span>
            </div>
            <div class="row">
                <div class="col-lg-4 col-sm-12 col-xs-12 sliced-box clearfix">
                    <div class="slide-con" :class='{"show-sliced1":showSliced1, "show-sliced2":showSliced2, "show-sliced3":showSliced3}'>
                        <div id="sliced-pie1" class="sliced-pie fl"></div>
                        <div id="sliced-pie2" class="sliced-pie fl"></div>
                        <div id="sliced-pie3" class="sliced-pie fl"></div>
                    </div>    
                </div>
                <div class="col-lg-4 col-sm-12 col-xs-12">
                    <div class="ct">A类产品比重：{{showSliced1 ? Number(slicedPieData1[0].value)*100 : showSliced2 ? Number(slicedPieData2[0].value)*100 : Number(slicedPieData3[0].value)*100}}%</div>
                    <div class="ct">B类产品比重：{{showSliced1 ? Number(slicedPieData1[1].value)*100 : showSliced2 ? Number(slicedPieData2[1].value)*100 : Number(slicedPieData3[1].value)*100}}%</div>
                    <div class="ct">C类产品比重：{{showSliced1 ? Number(slicedPieData1[2].value)*100 : showSliced2 ? Number(slicedPieData2[2].value)*100 : Number(slicedPieData3[2].value)*100}}%</div>
                    <div class="ct">D类产品比重：{{showSliced1 ? Number(slicedPieData1[3].value)*100 : showSliced2 ? Number(slicedPieData2[3].value)*100 : Number(slicedPieData3[3].value)*100}}%</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { DataSet } from '@antv/data-set';
if (process.browser) {
    const G2 = require('@antv/g2');
}
export default {
    data () {
      return {
        showhistogram:true,
        showSliced1:true,
        showSliced2:false,
        showSliced3:false,
        // slideNumber:['图一','图二','图三'],
        lineData:[
            { year: '1991', value: 3 },
            { year: '1992', value: 4 },
            { year: '1993', value: 3.5 },
            { year: '1994', value: 5 },
            { year: '1995', value: 4.9 },
            { year: '1996', value: 6 },
            { year: '1997', value: 7 },
            { year: '1998', value: 9 },
            { year: '1999', value: 1 }
        ],
        histogramData1:[
            { month: '1月', sales: 38 },
            { month: '2月', sales: 52 },
            { month: '3月', sales: 61 },
            { month: '4月', sales: 145 },
            { month: '5月', sales: 48 },
            { month: '6月', sales: 38 },
            { month: '7月', sales: 38 },
            { month: '8月', sales: 38 },
            { month: '9月', sales: 38 },
            { month: '10月', sales: 38 },
            { month: '11月', sales: 38 },
            { month: '12月', sales: 38 },   
        ],
        histogramData2:[
            { month: '1月', sales: 50 },
            { month: '2月', sales: 60 },
            { month: '3月', sales: 56 },
            { month: '4月', sales: 60 },
            { month: '5月', sales: 20 },
            { month: '6月', sales: 42 },
            { month: '7月', sales: 30 },
            { month: '8月', sales: 50 },
            { month: '9月', sales: 38 },
            { month: '10月', sales: 38 },
            { month: '11月', sales: 38 },
            { month: '12月', sales: 38 },  
        ],
        histogramAnalysis:["aaa","ssa","sas","sad","sa","asa","aaad","sadd","aa","asd","dass","saaad"],
        pieDate:[
            { item: '事例一', count: 40 },
            { item: '事例二', count: 21 },
            { item: '事例三', count: 17 },
            { item: '事例四', count: 13 },
            { item: '事例五', count: 9 }
        ],
        curveData:[
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
        slicedPieData1:[
            { value: 0.3, key: 'A类' },
            { value: 0.4, key: 'B类' },
            { value: 0.1, key: 'C类' },
            { value: 0.2, key: 'D类' },
        ],
        slicedPieData2:[
            { value: 0.3, key: 'A类' },
            { value: 0.2, key: 'B类' },
            { value: 0.2, key: 'C类' },
            { value: 0.3, key: 'D类' },
        ],
        slicedPieData3:[
            { value: 0.4, key: 'A类' },
            { value: 0.4, key: 'B类' },
            { value: 0.1, key: 'C类' },
            { value: 0.1, key: 'D类' },
        ],
      }
    },
    mounted(){
        if(process.browser){
            this.init();
        }
    },
    methods: {
        init(){
            //获取数据 绘制图表
            this.lineChart();
            this.histogramOne();
            this.histogramTwo();
            this.pieChart();
            this.curveChart();
            this.slicedPieChartOne();
            this.slicedPieChartTwo();
            this.slicedPieChartThree();
        },
        lineChart(){
            let chart = new G2.Chart({
                container: 'line-chart',
                forceFit: true,
                height: 300,
            });
            let data=this.lineData;
            chart.source(data);
            chart.scale('value', {
                min: 0
            });
            chart.scale('year', {
                range: [ 0 , 1 ]
            });
            chart.tooltip({
                crosshairs: {
                    type: 'line'
                }
            });
            chart.scale({
                value: {
                    alias: '数值' // 为属性定义别名
                }
            });
            chart.line().position('year*value').shape('smooth');
            chart.point().position('year*value').size(4).shape('circle').style({
                stroke: '#fff',
                lineWidth: 1
            });
            chart.render();
        },
        histogramOne(){
            const chart = new G2.Chart({
                container: 'histogram1',
                forceFit: true,
                height: 400
            });
            let data=this.histogramData1;
            chart.source(data);
            chart.scale('sales', {
                tickInterval: 20,
                alias: '销售额'
            });
            chart.axis('month',{
                label: {
                    textStyle: {
                        // fill: '#ccc',
                        // rotate: 90,
                    }
                }
            });
            
            chart.interval().position('month*sales');
            chart.render();
        },
        histogramTwo(){
            const chart = new G2.Chart({
                container: 'histogram2',
                forceFit: true,
                height: 400,
            });
            let data=this.histogramData2;
            chart.source(data);
            chart.scale('sales', {
                tickInterval: 20,
            });
            chart.interval().position('month*sales');
            chart.render();
        },
        pieChart(){
            const { DataView } = DataSet;
            const dv = new DataView();
            let data=this.pieDate;
            dv.source(data).transform({
                type: 'percent',
                field: 'count',
                dimension: 'item',
                as: 'percent'
            });
            const chart = new G2.Chart({
                container: 'pie-chart',
                forceFit: true,
                height: 300,
            });
            chart.source(dv, {
                percent: {
                    formatter: val => {
                        val = (val * 100) + '%';
                        return val;
                    }
                }
            });
            chart.coord('theta', {
                radius: 0.75
            });
            chart.tooltip({
                showTitle: false,
                itemTpl: '<li><span style="background-color:{color};" class="g2-tooltip-marker"></span>{name}: {value}</li>'
            });
            chart.intervalStack()
            .position('percent')
            .color('item')
            .label('percent', {
                formatter: (val, item) => {
                    return item.point.item + ': ' + val;
                }
            })
            .tooltip('item*percent', (item, percent) => {
                percent = percent * 100 + '%';
                return {
                    name: item,
                    value: percent
                };
            })
            .style({
                lineWidth: 1,
                stroke: '#fff'
            });
            chart.render();
        },
        curveChart(){
            const ds = new DataSet();
            let data= this.curveData;
            const dv = ds.createView().source(data);
            dv.transform({
                type: 'fold',
                fields: [ 'Tokyo', 'London' ], // 展开字段集
                key: 'city', // key字段
                value: 'temperature', // value字段
            });
            const chart = new G2.Chart({
                container: 'curve-chart',
                forceFit: true,
                height: 300
            });
            chart.source(dv, {
                month: {
                  range: [ 0, 1 ]
                },
            });
            chart.tooltip({
                crosshairs: {
                  type: 'line'
                }
            });
            chart.axis('temperature', {
                label: {
                    formatter: val => {
                        return val + '°C';
                    }
                }
            });
            chart.line().position('month*temperature').color('city').shape('smooth');
            chart.point().position('month*temperature').color('city').size(3).shape('circle').style({
                stroke: '#fff',
                lineWidth: 1
            });
            chart.render();
        },
        slicedPieChartOne(){
            const { DataView } = DataSet;
            const dv = new DataView();
            let data=this.slicedPieData1;
            dv.source(data)
                .transform({
                    type: 'percent',
                    field: 'value',
                    dimension: 'key',
                    as: 'percent'
                });
            const Shape = G2.Shape;
            Shape.registerShape('interval', 'burstPie', {
                getPoints(cfg) {
                    const width = cfg.size;
                    const x = cfg.x;
                    const min = cfg.y[0];
                    const max = cfg.y[1];
                    const res = [];
                    for (let i = 0; i < max; i += 0.1) {
                        if (min > i) {
                            continue;
                        } else if (min < i && min > i - 0.1) {
                            res.push(
                                { x: x - width / 2, y: min },
                                { x: x - width / 2, y: i - 0.01 },
                                { x: x + width / 2, y: i - 0.01 },
                                { x: x + width / 2, y: min }
                            );
                        }
                        const start = i;
                        const end = parseFloat((i + 0.1) > max ? max : i + 0.09);
                        res.push(
                            { x: x - width / 2, y: start },
                            { x: x - width / 2, y: end },
                            { x: x + width / 2, y: end },
                            { x: x + width / 2, y: start }
                        );
                    }
                    return res;
                },
                draw(cfg, container) {
                    // 将归一化后的数据转换为画布上的坐标
                    const points = cfg.origin.points;
                    let path = [];
                    for (let i = 0; i < cfg.origin.points.length; i += 4) {
                        path.push([ 'M', points[i].x, points[i].y ]);
                        path.push([ 'L', points[i + 1].x, points[i + 1].y ]);
                        path.push([ 'L', points[i + 2].x, points[i + 2].y ]);
                        path.push([ 'L', points[i + 3].x, points[i + 3].y ]);
                        path.push([ 'L', points[i].x, points[i].y ]);
                        path.push([ 'z' ]);
                    }
                    path = this.parsePath(path, true);
                    const shape = container.addShape('path', {
                        attrs: {
                            fill: cfg.color || '#00D9DF',
                            path,
                        },
                    });
                    return shape;
                }
            });
            const chart = new G2.Chart({
                container: 'sliced-pie1',
                forceFit: true,
                height: 300
            });
            chart.source(dv, {
                percent: {
                    formatter: val => {
                        return val * 100 + '%';
                    }
                }
            });
            // 重要：绘制饼图时，必须声明 theta 坐标系
            chart.coord('theta', {
                radius: 0.8, // 设置饼图的大小
                innerRadius: 0.7
            });
            chart.tooltip({
                showTitle: false
            });
            chart.axis('percent', {
                title: {
                    offset: 40,
                    text: '百分比'
                }
            });
            chart.intervalStack()
                .shape('burstPie')
                .position('percent')
                .color('key', [ '#1890ff', '#f04864', '#bfbfbf', '#123456']);
            chart.render();
        },
        slicedPieChartTwo(){
            const { DataView } = DataSet;
            const dv = new DataView();
            let data=this.slicedPieData2;
            dv.source(data)
                .transform({
                    type: 'percent',
                    field: 'value',
                    dimension: 'key',
                    as: 'percent'
                });
            const Shape = G2.Shape;
            Shape.registerShape('interval', 'burstPie', {
                getPoints(cfg) {
                    const width = cfg.size;
                    const x = cfg.x;
                    const min = cfg.y[0];
                    const max = cfg.y[1];
                    const res = [];
                    for (let i = 0; i < max; i += 0.1) {
                        if (min > i) {
                            continue;
                        } else if (min < i && min > i - 0.1) {
                            res.push(
                                { x: x - width / 2, y: min },
                                { x: x - width / 2, y: i - 0.01 },
                                { x: x + width / 2, y: i - 0.01 },
                                { x: x + width / 2, y: min }
                            );
                        }
                        const start = i;
                        const end = parseFloat((i + 0.1) > max ? max : i + 0.09);
                        res.push(
                            { x: x - width / 2, y: start },
                            { x: x - width / 2, y: end },
                            { x: x + width / 2, y: end },
                            { x: x + width / 2, y: start }
                        );
                    }
                    return res;
                },
                draw(cfg, container) {
                    // 将归一化后的数据转换为画布上的坐标
                    const points = cfg.origin.points;
                    let path = [];
                    for (let i = 0; i < cfg.origin.points.length; i += 4) {
                        path.push([ 'M', points[i].x, points[i].y ]);
                        path.push([ 'L', points[i + 1].x, points[i + 1].y ]);
                        path.push([ 'L', points[i + 2].x, points[i + 2].y ]);
                        path.push([ 'L', points[i + 3].x, points[i + 3].y ]);
                        path.push([ 'L', points[i].x, points[i].y ]);
                        path.push([ 'z' ]);
                    }
                    path = this.parsePath(path, true);
                    const shape = container.addShape('path', {
                        attrs: {
                            fill: cfg.color || '#00D9DF',
                            path,
                        },
                    });
                    return shape;
                }
            });
            const chart = new G2.Chart({
                container: 'sliced-pie2',
                forceFit: true,
                height: 300
            });
            chart.source(dv, {
                percent: {
                    formatter: val => {
                        return val * 100 + '%';
                    }
                }
            });
            // 重要：绘制饼图时，必须声明 theta 坐标系
            chart.coord('theta', {
                radius: 0.8, // 设置饼图的大小
                innerRadius: 0.7
            });
            chart.tooltip({
                showTitle: false
            });
            chart.axis('percent', {
                title: {
                    offset: 40,
                    text: '百分比'
                }
            });
            chart.intervalStack()
                .shape('burstPie')
                .position('percent')
                .color('key', [ '#1890ff', '#f04864', '#bfbfbf', '#123456']);
            chart.render();
        },
        slicedPieChartThree(){
            const { DataView } = DataSet;
            const dv = new DataView();
            let data=this.slicedPieData3;
            dv.source(data)
                .transform({
                    type: 'percent',
                    field: 'value',
                    dimension: 'key',
                    as: 'percent'
                });
            const Shape = G2.Shape;
            Shape.registerShape('interval', 'burstPie', {
                getPoints(cfg) {
                    const width = cfg.size;
                    const x = cfg.x;
                    const min = cfg.y[0];
                    const max = cfg.y[1];
                    const res = [];
                    for (let i = 0; i < max; i += 0.1) {
                        if (min > i) {
                            continue;
                        } else if (min < i && min > i - 0.1) {
                            res.push(
                                { x: x - width / 2, y: min },
                                { x: x - width / 2, y: i - 0.01 },
                                { x: x + width / 2, y: i - 0.01 },
                                { x: x + width / 2, y: min }
                            );
                        }
                        const start = i;
                        const end = parseFloat((i + 0.1) > max ? max : i + 0.09);
                        res.push(
                            { x: x - width / 2, y: start },
                            { x: x - width / 2, y: end },
                            { x: x + width / 2, y: end },
                            { x: x + width / 2, y: start }
                        );
                    }
                    return res;
                },
                draw(cfg, container) {
                    // 将归一化后的数据转换为画布上的坐标
                    const points = cfg.origin.points;
                    let path = [];
                    for (let i = 0; i < cfg.origin.points.length; i += 4) {
                        path.push([ 'M', points[i].x, points[i].y ]);
                        path.push([ 'L', points[i + 1].x, points[i + 1].y ]);
                        path.push([ 'L', points[i + 2].x, points[i + 2].y ]);
                        path.push([ 'L', points[i + 3].x, points[i + 3].y ]);
                        path.push([ 'L', points[i].x, points[i].y ]);
                        path.push([ 'z' ]);
                    }
                    path = this.parsePath(path, true);
                    const shape = container.addShape('path', {
                        attrs: {
                            fill: cfg.color || '#00D9DF',
                            path,
                        },
                    });
                    return shape;
                }
            });
            const chart = new G2.Chart({
                container: 'sliced-pie3',
                forceFit: true,
                height: 300
            });
            chart.source(dv, {
                percent: {
                    formatter: val => {
                        return val * 100 + '%';
                    }
                }
            });
            // 重要：绘制饼图时，必须声明 theta 坐标系
            chart.coord('theta', {
                radius: 0.8, // 设置饼图的大小
                innerRadius: 0.7
            });
            chart.tooltip({
                showTitle: false
            });
            chart.axis('percent', {
                title: {
                    offset: 40,
                    text: '百分比'
                }
            });
            chart.intervalStack()
                .shape('burstPie')
                .position('percent')
                .color('key', [ '#1890ff', '#f04864', '#bfbfbf', '#123456']);
            chart.render();
        },
        sliced1(){
            this.showSliced1=true;
            this.showSliced2=false;
            this.showSliced3=false;
        },
        sliced2(){
            this.showSliced1=false;
            this.showSliced2=true;
            this.showSliced3=false;
        },
        sliced3(){
            this.showSliced1=false;
            this.showSliced2=false;
            this.showSliced3=true;
        },
    },

}
</script>

<style>
*{
    padding: 0;
    margin:0;
}
.fl{
    float: left;
}
.fr{
    float: right;
}
.clearfix:after {
    content: "";
    display: block;
    clear: both;
}
.clearfix {
    zoom: 1;
}
.ct{
    text-align: center;
}
.cu{
    cursor: pointer;
}
canvas{
    width: 100% !important;
    height:100% !important;
}
.row{
    margin: 0;
}
.histogram-wrap{
    /*padding: 20px;*/
    box-sizing: border-box;
}
.histogram-top{
    line-height: 50px;
}
.histogram-con-wrap{
    overflow: hidden;
}
.histogram-lt{
    overflow: hidden;
}
.histogram-con{
    width: 200%;
}
#histogram1{
    width:50%;
}
#histogram2{
    width: 50%;
}
.showlt{
    transform: translateX(0);
    transition: all .3s linear;
}
.showrt{
    transform: translateX(-50%);
    transition: all .3s linear;
}
/*.sliced-box-wrap{
    position: relative;
}*/
.sliced-box{
    /*position: relative;*/
    overflow: hidden;
}
.slide-con{
    /*position: absolute;*/
    width: 300%;
}
.sliced-pie{
    width: 33.33333333333%;
}
.show-sliced1{
    transform: translateX(0);
}
.show-sliced2{
    transform: translateX(-33.33333333333%);
}
.show-sliced3{
    transform: translateX(-66.66666666666%);
}


</style>
