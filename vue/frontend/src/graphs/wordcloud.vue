<template>
    <div class="wordcloud" id="wordcloud"></div>
</template>
<script>
    import $search from './../util/search'
    // import $drawGraphs from "./../util/drawGraphs.js"
    require('echarts-wordcloud');
    let echarts = require('echarts');
    export default {
        name: "wordcloud",
        props: {
            items: {
                type: Array,
                default () {
                    return {
                        "test": "test"
                    }
                }
            }
        },
        data() {
            return {
                // tabItem:"Paper"
            }
        },
        mounted() {
            this.start_draw();
        },
        methods: {
            start_draw() {
                let id = 'wordcloud'
                this.draw_wordcloud(id,this.items);
            },
            draw_wordcloud(id,data) {
                let chart = document.getElementById(id);
                let myChart = echarts.init(chart);
                let d=[]
                // console.log(data)
              //  d.push(data)
                 for(let i=0;i<data.length;i++) {
                     var t={}
                     console.log(data[i]['name']+","+data[i]['value'])
                     t['name'] = data[i]['name']
                     t['value'] = data[i]['value'];
                     d[i]=t
                 }
                // console.log(d)
                const option = {
                    tooltip: {
                        trigger: 'item',
                        // formatter:
                    },
                    toolbox: {
                        show: false,
                        orient: 'vertical',
                        x: 'right',
                        y: 'center',
                    },
                    series: [{
                        name: 'WordCloud',
                        type: 'wordCloud',
                        size: ['90%', '90%'],
                        textRotation : [-90, 90],
                        shape: 'circle',
                        textStyle: {
                            normal:{
                                // color() {
                                //     return `rgb(${[
                                //         Math.round(Math.random()*160),
                                //         Math.round(Math.random()*160),
                                //         Math.round(Math.random()*160)
                                //     ].join(',')})`
                                // }
                                color: () => {
                                    let testcolor = ["rgb(129, 212, 250)","rgb(239, 154, 154)","rgb(128, 203, 196)","rgb(129, 199, 132)","rgb(156, 204, 101)","rgb(212, 225, 87)","rgb(255, 213, 79)","rgb(255, 167, 38)","rgb(255, 110, 64)","rgb(161, 136, 127)","rgb(248, 187, 208)","rgb(128, 203, 196)","rgb(209, 196, 233)"]
                                    let num = Math.floor(Math.random() * testcolor.length);
                                    return testcolor[num]
                                }
                            },
                            emphasis:{
                                shadowBlur:10,
                                shadowColor:'#999'
                            }
                        },
                        autoSize: {
                            enable: true,
                            minSize: 14
                        },
                        data: d//[{name:data['name'],value:data['value']}]
                    }],
                };
                myChart.setOption(option);
                window.addEventListener('resize', function () {
                    // initial();
                    myChart.resize()
                })
                myChart.on("click",function(params){
                    // $router.push({
                    //     path:"/displayInfo",
                    //     query:{query:params['data']['name'].toLowerCase()}
                    // })
                    let query = params['data']['name'].toLowerCase()
                    $search.to_display(query)
                })
            }
        }
    }
</script>
<style>
    .wordcloud {
        min-width:100px;
        height:300px;
    }
</style>