<template>
    <el-card class="box-card" shadow="hover">
        <div class="wordcloud" id="icloud"></div>
    </el-card>
</template>
<script>
    import $search from '../../util/search'
    require('echarts-wordcloud');
    let echarts = require('echarts');
    export default {
        props:['wordcloud'],
        data() {
            return {
                k:[]
            }
        },
        mounted() {
        },
        watch:{
          wordcloud:function (newData) {
                this.k = newData
                this.start_draw()
          }
        },
        methods: {
            start_draw() {
                let id = 'icloud'
                this.draw_wordcloud(id,this.k);
            },
            draw_wordcloud(id,data) {
              let chart = document.getElementById(id);
                let myChart = echarts.init(chart);
                let d = []
                let index=0
                let color = ["#577590","#43AA8B","#E76F51","#F4A261","#E9C46A"]
                let colorset = ["rgb(129, 212, 250)","rgb(239, 154, 154)","rgb(128, 203, 196)","rgb(129, 199, 132)","rgb(156, 204, 101)","rgb(212, 225, 87)","rgb(255, 213, 79)","rgb(255, 167, 38)","rgb(255, 110, 64)","rgb(161, 136, 127)","rgb(248, 187, 208)","rgb(128, 203, 196)","rgb(209, 196, 233)"]
                for(let key in data) {
                    let tempData = data[key]
                    if(!isNaN(key)) {
                        for(let i=0;i<data.length;i++) {
                            var t={}
                            let j=Math.floor(Math.random()*10)
                            t['name'] = data[i]['Mention']
                            t['value'] = data[i]['occurences'];
                            t['textStyle'] = {
                                normal: {
                                    color: colorset[j]
                                },
                                emphasis: {
                                    shadowBlur: 10,
                                    shadowColor: '#999'
                                }
                            }
                            d[i]=t
                        }
                        break;
                    }
                    else {
                      if (tempData!=null){
                        for (let i = 0; i < tempData.length; i++) {
                          var map = {}
                          map['name'] = tempData[i]['Mention']
                          map['value'] = tempData[i]['occurences']
                          map['id'] = tempData[i]['id']
                          map['textStyle'] = {
                            normal: {
                              color: color[index]
                            },
                            emphasis: {
                              shadowBlur: 10,
                              shadowColor: '#999'
                            }
                          }
                          d.push(map)
                        }
                      }

                        index++;
                    }

                }
                const option = {
                    tooltip: {
                        trigger: 'item',
                    },
                    toolbox: {
                        show: false,
                        orient: 'vertical',
                        x: 'right',
                        y: 'center',
                    },
                    series: [{
                        left: 'center',
                        top: 'center',
                        width: '90%',
                        height: '90%',
                        right: null,
                        bottom: null,
                        name: 'wordcloud',
                        type: 'wordCloud',
                      sizeRange: [15, 40],
                        textRotation : [-90, 90],
                        rotationStep: 30,
                        gridSize: 3,
                        shape: 'square',
                        drawOutOfBound: false,
                        textStyle: {
                            normal:{
                                fontWeight: 'bold',
                            },
                            emphasis:{
                                fontWeight:'bolder',
                                fontSize:40,
                                shadowBlur:55,
                                shadowColor:'#999'
                            }
                        },
                        autoSize: {
                            enable: true,
                            minSize: 5
                        },
                        data: d
                    }],
                };
                myChart.setOption(option);
                window.addEventListener('resize', function () {
                    myChart.resize()
                })
                myChart.on("click",function(params){
                    let query = params['data']['id']
                    $search.display_bioentity(query)
                })
            }
        }
    }
</script>
<style>
    .wordcloud {
      min-height: 300px;
        max-width:450px;
        max-height:450px;
        padding: 0px ;
    }
    .box-card {
        width: 330px;
        margin-top: 2.5rem;
        padding: 0px;
    }
    .el-card__body{
        padding: 0 0 0 0;
    }
</style>