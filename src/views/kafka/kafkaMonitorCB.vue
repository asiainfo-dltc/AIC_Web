<template>
    <div style="margin: 20px;">
        <div>
            <Row style="margin-bottom: 25px;">
                <Col span="12">消费组：
                    <Select v-model="groupIds"  multiple style="width: 400px">
                        <Option v-for="item in menuList" :value="item.value" :key="item.value">{{ item.label }}</Option>
                    </Select>
                </Col>
                <Col span="8"><Button type="primary" shape="circle" icon="ios-search" @click="getLags()">搜索</Button></Col>
            </Row>
        </div>
        <div >
            <div style="padding: 10px 0;">
                        <template>
                            <Table :columns="columns10" :data="result"></Table>
                        </template>
            </div>
        </div>
        <Divider/>
        <div id="main" style="width:100%;height:600px;"></div>
        <Modal v-model="offsetModal" width="500" title="偏移量设置" @on-ok="settingOk()"  >
            <Form  :label-width="80" >
                <Form-item label="topic:">
                    <Input v-model="topic"  type="text" style="width: 204px" disabled/>
                </Form-item>

                <Form-item label="partition:" prop="partition" >
                    <Input v-model="partition" type="text" disabled></Input>
                </Form-item>
                <Form-item label="groupId:" prop="groupId">
                    <Input v-model="groupId" type="text" disabled></Input>
                </Form-item>
                <Form-item label="currentOffset:" prop="currentOffset">
                    <Input v-model="currentOffset" type="text" ></Input>
                </Form-item>
            </Form>
        </Modal>
    </div>
</template>
<script>
    import expandRow from '../../template/table-expand.vue';
    export default {
        components: { expandRow },
        data () {
            return {
                /*消费组*/

                /*选中的消费组*/
                groupIds:['T0000_CB.DOMAIN1.ROUTER.ACCESS_0630_221',
                      'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221',
                    'T0000_CB.DOMAIN3.ROUTER.ACCESS_0630_221',
                    'T0000_CB.DOMAIN4.ROUTER.ACCESS_0630_221',
                    'T0000_CB.DOMAIN5.ROUTER.ACCESS_0630_221',
                    'T0000_CB.DOMAIN6.ROUTER.ACCESS_0630_221',
                    'T0000_CB.DOMAIN7.ROUTER.ACCESS_0630_221',
                    'T0000_CB.DOMAIN8.ROUTER.ACCESS_0630_221',

                ],
                topic:null,
                groupId:null,
                partition:null,
                currentOffset:null,
                offsetModal:false,
                legend:[],
                xAxis:[],
                series:[],
                topics:[],
                logEndOffset:[],
                /*分页total属性绑定值*/
                total:0,
                /*loading*/
                loading: true,
                /*pageInfo实体*/
                pageInfo:{
                    page:0,
                    pageSize:10
                },
                columns10: [
                    {
                        type: 'expand',
                        width: 50,
                        render: (h, params) => {
                            return h(expandRow, {
                                props: {
                                    row: params.row,
                                    columns:this.columns1
                                }
                            })
                        }
                    },
                    {
                        title: 'groupId',
                        key: 'groupId'
                    },
                    {
                        title: 'topic',
                        key: 'topic'
                    }
                ],
                result:[],
                /*菜单列表*/
                menuList:[{
                    label:"T0000_CB.DOMAIN1.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN1.ROUTER.ACCESS_0630_221"
                }, {
                        label:"T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221"
                    },
                    {
                        label:"T0000_CB.DOMAIN3.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN3.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN4.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN4.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN5.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN5.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN6.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN6.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN7.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN7.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN8.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN8.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN1.MAIN.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN1.MAIN.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN2.MAIN.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN2.MAIN.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN3.MAIN.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN3.MAIN.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN4.MAIN.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN4.MAIN.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN5.MAIN.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN5.MAIN.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN6.MAIN.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN6.MAIN.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN7.MAIN.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN7.MAIN.ROUTER.ACCESS_0630_221"
                    },{
                        label:"T0000_CB.DOMAIN8.MAIN.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN8.MAIN.ROUTER.ACCESS_0630_221"
                    }
                    ],
                /*生产类型表显示字段*/
                columns1: [

                    {
                        title: 'partition',
                        key: 'partition',
                        sortable: true
                    },
                    {
                        title: 'currentOffset',
                        key: 'currentOffset',
                        sortable: true
                    },
                    {
                        title: 'logEndOffset',
                        key: 'logEndOffset',
                        sortable: true
                    },
                    {
                        title: 'lag',
                        key: 'lag',
                        sortable: true
                    },
                    {
                        title: 'tableName',
                        key: 'tableName',
                        sortable: false
                    },
                    {
                        title: 'operationTime',
                        key: 'operationTime'
                    },
                    {
                        title: '调整偏移量',
                        slot: 'action',
                        render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'info',
                                    },
                                    on: {
                                        click: () => {
                                            this.seek(params.row);
                                        }
                                    }
                                },'seek')
                            ]);
                        }
                    }
                ],
                /*生产类型表数据*/
                data1: []
            }
        },
        mounted(){
            /*页面初始化调用方法*/
           this.getLagsHis();


                   /* var option = {
                        title: {
                            text: 'ECharts 入门示例'
                        },
                        tooltip: {},
                        legend: {
                            data:['销量']
                        },
                        xAxis: {
                            data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
                        },
                        yAxis: {},
                        series: [{
                            name: '销量',
                            type: 'bar',
                            data: [5, 20, 36, 10, 10, 20]
                        }]
                    };*/

                    // 使用刚指定的配置项和数据显示图表。

            /*this.axios({
                method: 'get',
                url: '/menus/parentId',
                params: {
                    'parentId': 0
                }
            }).then(function (response) {
                var listTemp = response.data;
                for (var i = 0; i < listTemp.length; i++) {
                    this.menuList.push({
                        "value": listTemp[i].id,
                        "label": listTemp[i].name
                    });
                }
            }.bind(this)).catch(function (error) {
                alert(error);
            });*/
        },
        methods:{

            /*得到表数据*/
            getLags() {

                //let groupIds=this.groupIds;
                //alert(this.groupIds);
                this.axios({
                    method: 'get',
                    url: '/getLags',
                    async:false,
                    params: {
                        "groupIds":JSON.stringify(this.groupIds)
                    }
                }).then(function (response) {
                   // alert("result"+response.data);
                    this.result=response.data;
                    /*this.data1=response.data.data;
                    this.total=response.data.totalCount;*/
                }.bind(this)).catch(function (error) {
                    alert(error);
                });
            },
            getLagsHis(){
                let myChart = this.$echarts.init(document.getElementById('main'));
                //获取数据
                this.axios({
                    method: 'get',
                    url: '/getLagsHis',
                    async:false
                    /*params: {
                         "groupIds":JSON.stringify(this.groupIds)
                     }*/
                }).then(function (response) {
                    // alert("result"+response.data);
                    //   this.result=response.data;
                    option.legend.data=response.data.legend;
                    option.xAxis.data=response.data.xAxis;
                    option.series=response.data.series;
                    myChart.setOption(option);
                }.bind(this)).catch(function (error) {
                    alert(error);
                });

                let option = {
                    /*      title: {
                              text: 'TOPIC数据量'
                          },*/
                    tooltip: {
                        trigger: 'axis'
                    },
                    legend: {
                        data:[]
                    },
                    grid: {
                        left: '3%',
                        right: '4%',
                        bottom: '3%',
                        containLabel: true
                    },
                    toolbox: {
                        feature: {
                            saveAsImage: {}
                        }
                    },
                    xAxis: {
                        type: 'category',
                        boundaryGap: false,
                        data:  this.xAxis
                    },
                    yAxis: {
                        type: 'value'
                    },
                    // series: this.series
                    series: []
                };
            },
            seek(e){
                this.groupId=e.groupId;
                this.topic=e.topic;
                this.partition=e.partition;
                this.currentOffset=e.currentOffset;
                this.offsetModal = true;

            },
            settingOk(){
               // alert("currentOffset"+this.currentOffset);

                this.axios({
                    method: 'post',
                    url: '/seekOffset',
                    async:false,
                    data: {
                        "groupId":this.groupId,
                        "partition":this.partition,
                        "currentOffset":this.currentOffset,
                        "topic":this.topic
                    }
                }).then(function (response) {
                    // alert("result"+response.data);
                    this.result=response.data;
                    /*this.data1=response.data.data;
                    this.total=response.data.totalCount;*/
                }.bind(this)).catch(function (error) {
                    alert(error);
                });
            }
        }
    }
</script>