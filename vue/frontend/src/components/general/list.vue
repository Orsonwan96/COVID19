<template>
    <el-tabs type="card" :tab-position="tabPosition" stretch="true" >
        <el-tab-pane id="tab1" class="tab" label="Disease" >
            <el-table
                    :data="tab1"
                    style="width: 100%"
                    :show-header="false"
                    :row-class-name="tableRowClassName2"
                    :border="false"
                    :height="200"
                    :row-style="{height:'40px'}"
                    :cell-style="{padding:'0px'}"
            >
                <el-table-column
                        prop="entity"
                        label="Co-Institutions"
                        align="center"
                >
                    <template slot-scope="scope">
                        <a :href="scope.row.entity.url" style="color:#000000">{{scope.row.entity.name}}</a>
                    </template>
                </el-table-column>
                <el-table-column
                        prop="num"
                        label="Co-Institutions"
                        align="center"
                >
                </el-table-column>
            </el-table>
        </el-tab-pane>
        <el-tab-pane id="tab2" class="tab" label="Drug">
            <el-table
                    :data="tab2"
                    style="width: 100%"
                    :show-header="false"
                    :row-class-name="tableRowClassName2"
                    :border="false"
                    :height="200"
                    :row-style="{height:'40px'}"
                    :cell-style="{padding:'0px'}"
            >
                <el-table-column
                        prop="entity"
                        label="Co-Institutions"
                        align="center"
                >
                    <template slot-scope="scope">
                        <a :href="scope.row.entity.url" style="color:#000000">{{scope.row.entity.name}}</a>
                    </template>
                </el-table-column>
                <el-table-column
                        prop="num"
                        label="Co-Institutions"
                        align="center"
                >
                </el-table-column>
            </el-table>
        </el-tab-pane>
        <el-tab-pane id="tab3" class="tab" label="Gene">
            <el-table
                    :data="tab3"
                    style="width: 100%"
                    :show-header="false"
                    :row-class-name="tableRowClassName2"
                    :border="false"
                    :height="200"
                    :row-style="{height:'40px'}"
                    :cell-style="{padding:'0px'}"
            >
                <el-table-column
                        prop="entity"
                        label="Co-Institutions"
                        align="center"
                >
                    <template slot-scope="scope">
                        <a :href="scope.row.entity.url" style="color:#000000">{{scope.row.entity.name}}</a>
                    </template>
                </el-table-column>
                <el-table-column
                        prop="num"
                        label="Co-Institutions"
                        align="center"
                >
                </el-table-column>
            </el-table>
        </el-tab-pane>
    </el-tabs>
</template>

<script>
    export default {

        props:['entity'],
        data() {
            return {
                d:[],
                tabPosition:"top",
                tab1:[],
                tab2:[],
                tab3:[],
            }
        },
        mounted() {
        },
        watch: {
          entity:function (newData) {
              this.d = newData
              this.get_data()
          }
        },
        methods: {
            get_data() {
                    let i=1;
                    for(let key in this.d) {
                      let temp = this.d[key]
                      if (this.d[key] != null) {
                        temp = this.d[key]
                      }

                      let mention = []

                      if (temp!==null) {
                        for (let i = 0; i < temp.length; i++) {
                          mention.push({
                            'entity': {
                              name: temp[i]['Mention'],
                              'url': "/COVID19/#/bioentity/" + temp[i]['id']
                            },
                            'num': temp[i]['occurences']
                          })
                        }
                      }


                        console.log(mention)
                        if(i===1) {
                            this.tab1 = mention
                        } else if(i===2) {
                            this.tab2 = mention
                        } else if(i===3) {
                            this.tab3 = mention
                        } else if(i===4) {
                            this.tab4 = mention
                        } else if(i===5) {
                            this.tab5 = mention
                            console.log('mention5')
                        }
                        i++;
                    }
            },
            tableRowClassName({row,rowIndex}) {
                var r=row
                r=0
                console.log(r)
                if(rowIndex%2===1) {
                    return 'white-background'
                } else {
                    return 'gray-background'
                }
            },
            tableRowClassName2({row,rowIndex}) {
                var r=row
                r=0
                console.log(r)
                if(rowIndex%2===0) {
                    return 'white-background'
                } else {
                    return 'gray-background'
                }
            }
        }

    }
</script>

<style scoped>
    /deep/ .el-table .gray-background {
        background: #f2f2f2;
    }
    /deep/ .el-table .white-background {
        background: #ffffff;
    }
    .entity-name {
        /*text-decoration:underline;*/
        color: black;
    }
    .el-tabs el-tabs--left{
        float: left;
        margin-bottom: 0;
        margin-right: 0;
    }
    .el-tabs .el-tabs__header.is-left {
        float: left;
        margin-bottom: 0;
        margin-right: 0;
    }
    .scroll{
      overflow-y: scroll;
      height: 300px;
    }
    .scroll::-webkit-scrollbar {
      -webkit-appearance: none;
      width: 7px;
    }

    .scroll::-webkit-scrollbar-thumb {
      border-radius: 4px;
      background-color: rgba(0, 0, 0, .5);
      box-shadow: 0 0 1px rgba(255, 255, 255, .5);
    }
</style>