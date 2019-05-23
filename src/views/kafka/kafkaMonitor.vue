<template>
    <div style="margin: 20px;">
        <div>
            <Row style="margin-bottom: 25px;">
                <Col span="12">消费组：
                    <Select v-model="groupIds"  multiple style="width: 400px">
                        <Option v-for="item in menuList" :value="item.value" :key="item.value">{{ item.label }}</Option>
                    </Select>
                </Col>

            </Row>
            <Row style="margin-bottom: 25px;">
                <Col span="8">时间：
                    <DatePicker type="date" placeholder="Select date" style="width: 200px"></DatePicker>
                </Col>
                <Col span="8"><Button type="primary" shape="circle" icon="ios-search" @click="getLags()">搜索</Button></Col>
                </Row>
        </div>
        <div >
            <div style="padding: 10px 0;">
                        <template>
                            <Table :columns="columns10" :data="data9"></Table>
                        </template>
<!--
                        <Table border :columns="columns1" :data="data1"  @on-selection-change="s=>{change(s)}" @on-row-dblclick="s=>{dblclick(s)}"></Table>
-->
            </div>
                   <!-- <div style="text-align: right;">
                        <Page :total="total" :page-size="pageInfo.pageSize" show-elevator show-total @on-change="e=>{pageSearch(e)}"></Page>
                    </div>-->
        </div>
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
                'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221'],
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
                data9: [
                   /* {
                        groupId: 'T0000_CB.DOMAIN1.ROUTER.ACCESS_0630_221',
                        topic: 18,
                        detail: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]

                    },
                    {
                        groupId: 'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221',
                        topic: 25,
                        detail: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]
                    }
                    ,
                    {
                        groupId: 'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221',
                        topic: 25,
                        detail: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]
                    }
                    ,
                    {
                        groupId: 'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221',
                        topic: 25,
                        address: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]
                    },
                    {
                        groupId: 'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221',
                        topic: 25,
                        address: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]
                    },
                    {
                        groupId: 'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221',
                        topic: 25,
                        address: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]
                    },
                    {
                        groupId: 'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221',
                        topic: 25,
                        address: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]
                    },
                    {
                        groupId: 'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221',
                        topic: 25,
                        address: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]
                    },
                    {
                        groupId: 'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221',
                        topic: 25,
                        address: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]
                    },
                    {
                        groupId: 'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221',
                        topic: 25,
                        address: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]
                    }*/
                ],
                /*菜单列表*/
                menuList:[{
                    label:"T0000_CB.DOMAIN1.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN1.ROUTER.ACCESS_0630_221"
                }, {
                        label:"T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221",value:"T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221"
                    },
                    {
                        label:"T0000_CB.DOMAIN3.ROUTER.ACCESS_0630_221",value:"3"
                    },{
                        label:"T0000_CB.DOMAIN4.ROUTER.ACCESS_0630_221",value:"4"
                    },{
                        label:"T0000_CB.DOMAIN5.ROUTER.ACCESS_0630_221",value:"5"
                    },{
                        label:"T0000_CB.DOMAIN6.ROUTER.ACCESS_0630_221",value:"6"
                    },{
                        label:"T0000_CB.DOMAIN7.ROUTER.ACCESS_0630_221",value:"7"
                    },{
                        label:"T0000_CB.DOMAIN8.ROUTER.ACCESS_0630_221",value:"8"
                    },{
                        label:"T0000_CB.DOMAIN1.MAIN.ROUTER.ACCESS_0630_221",value:"9"
                    },{
                        label:"T0000_CB.DOMAIN2.MAIN.ROUTER.ACCESS_0630_221",value:"10"
                    },{
                        label:"T0000_CB.DOMAIN3.MAIN.ROUTER.ACCESS_0630_221",value:"11"
                    },{
                        label:"T0000_CB.DOMAIN4.MAIN.ROUTER.ACCESS_0630_221",value:"12"
                    },{
                        label:"T0000_CB.DOMAIN5.MAIN.ROUTER.ACCESS_0630_221",value:"13"
                    },{
                        label:"T0000_CB.DOMAIN6.MAIN.ROUTER.ACCESS_0630_221",value:"14"
                    },{
                        label:"T0000_CB.DOMAIN7.MAIN.ROUTER.ACCESS_0630_221",value:"15"
                    },{
                        label:"T0000_CB.DOMAIN8.MAIN.ROUTER.ACCESS_0630_221",value:"16"
                    }


                    ],
                /*生产类型表显示字段*/
                columns1: [

                    {
                        title: 'partition',
                        key: 'partition'
                    },
                    {
                        title: 'currentOffset',
                        key: 'currentOffset'
                    },
                    {
                        title: 'logEndOffset',
                        key: 'logEndOffset'
                    },
                    {
                        title: 'lag',
                        key: 'lag'
                    }
                    ,
                    {
                        title: 'operationTime',
                        key: 'operationTime'
                    }
                ],
                /*生产类型表数据*/
                data1: []
            }
        },
        mounted(){
            /*页面初始化调用方法*/
            this.getLags();

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
                alert(this.groupIds);
                this.axios({
                    method: 'get',
                    url: '/getLags',
                    params: {
                        "groupIds":JSON.stringify(this.groupIds)
                    }
                }).then(function (response) {
                    alert("result"+response.data);
                    this.data9=response.data;
                    /*this.data1=response.data.data;
                    this.total=response.data.totalCount;*/
                }.bind(this)).catch(function (error) {
                    alert(error);
                });
            }
        }
    }
</script>