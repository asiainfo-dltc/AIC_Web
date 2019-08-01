
<template>
    <div  style="margin: 20px;">
         表名：ORDER_RELATION_TABLE
        <div>

            order_line_id：
            <Input  v-model="tradeId" type="text" placeholder="请输入本系统tradeId" style="width: 200px;margin-right: 50px"></Input>
            sys_code:
            <Input  v-model="sysCode01" type="text" placeholder="请输入本系统sys_code" style="width: 200px;margin-right: 50px"></Input>


        </div>
        <div style="margin: 20px">
            RowKey:
            <Input  v-model="relationRowKey"  type="text" placeholder="" style="width: 400px;margin-right: 50px"></Input>
            <Button type="primary" shape="circle"  @click="generateRelation()">generate</Button>
        </div>
        <div >
            <div style="padding: 10px 0;">
                <template>
                    <Table :columns="columnRelation" :data="resultRelation"></Table>
                </template>
            </div>
        </div>
        <Divider />

        表名：STANDARD_OC_ORDER
        <div>
            PK：
            <Input  v-model="pk" type="text" placeholder="请输入PK:例${order_id}|${ocder_line_id}" style="width: 200px;margin-right: 50px"></Input>
            sys_code:
            <Input  v-model="sysCode02" type="text" placeholder="请输入sys_code" style="width: 200px;margin-right: 50px"></Input>

        </div>
        <div style="margin: 20px">
            RowKey:
            <Input  v-model="standardRowKey"  type="text" placeholder="" style="width: 400px;margin-right: 50px"></Input>
            <Button type="primary" shape="circle"  @click="generateStandard()">generate</Button>
        </div>
        <div >
            <div style="padding: 10px 0;">
                <template>
                    <Table :columns="columnStandard" :data="resultStandard"></Table>
                </template>
            </div>
        </div>
        <Divider />
        表名：OC_ORDER
        <div>
            PK：
            <Input  v-model="tradeIdTop" type="text" placeholder="请输入顶层系统tradeId" style="width: 200px;margin-right: 50px"></Input>
            sys_code:
            <Input  v-model="sysCode03" type="text" placeholder="请输入顶层sys_code" style="width: 200px;margin-right: 50px"></Input>

        </div>
        <div style="margin: 20px">
            RowKey:
            <Input  v-model="ocRowKey"  type="text" placeholder="" style="width: 400px;margin-right: 50px"></Input>
            <Button type="primary" shape="circle"  @click="generateOC()">generate</Button>
        </div>
        <div >
            <div style="padding: 10px 0;">
                <template>
                    <Table :columns="columnOC" :data="resultOC"></Table>
                </template>

            </div>
        </div>


        <!--<div id="main" style="width: 600px;height:400px;"></div>-->

    </div>

</template>
<!--
<script src="../../libs/sha1.js"></script>
-->


<script>
import sha1 from "../../libs/sha1.js";
//import echarts from 'echarts';
export default {

        data(){
            return {
                tradeId:null,
                tradeIdTop:null,
                sysCode01:null,
                sysCode02:null,
                sysCode03:null,
                relationRowKey:null,
                standardRowKey:null,
                ocRowKey:null,
                resultRelation:[],
                columnRelation:[],

                resultStandard:[],
                columnStandard:[],

                resultOC:[],
                columnOC:[],
                tableName:null,
                pk:null

            }
        },
    mounted(){
        /*let myChart = this.$echarts.init(document.getElementById('main'));
        //let myChart = echarts.init(document.getElementById('main'));
        var option = {
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
        };

        // 使用刚指定的配置项和数据显示图表。
        myChart.setOption(option);*/
},
        methods: {
            generateRelation(){
                if(this.tradeId=="")
                {
                    alert("请输入本系统tradeId");
                }else if(this.sysCode01==""){
                    alert("请输入sys_code");
                }else{

               let sha= sha1.hex_sha1(this.tradeId);
               this.relationRowKey=sha.substring(sha.length-3)+this.sysCode01+sha;
                    this.axios({
                        method: 'get',
                        url: '/getRelationData',
                        async: false,
                        params: {
                            "tableName":'ORDER_RELATION_TABLE',
                            "rowKey": this.relationRowKey
                        }
                    }).then(function (response) {
                        let obj=response.data[0];
                        let columns=[];
                        Object.keys(obj).forEach(function(key){
                            console.log(key,obj[key]);
                            columns.push({"title":key,"key":key});
                        });
                        this.columnRelation=columns;
                        this.resultRelation = response.data;
                       // alert(this.result);
                        /*this.data1=response.data.data;
                    this.total=response.data.totalCount;*/
                    }.bind(this)).catch(function (error) {
                        alert(error);
                    });

                }

            },
            generateStandard(){
                if(this.pk=="")
                {
                    alert("请输入主键");
                }else if(this.sysCode02==""){
                    alert("请输入sys_code");
                }else{

                    let sha= sha1.hex_sha1(this.pk);
                    this.standardRowKey=sha.substring(sha.length-3)+this.sysCode02+sha;
                    this.axios({
                        method: 'get',
                        url: '/getRelationData',
                        async: false,
                        params: {
                            "tableName":'STANDARD_OC_ORDER',
                            "rowKey": this.standardRowKey
                        }
                    }).then(function (response) {
                        let obj=response.data[0];
                        let columns=[];
                        Object.keys(obj).forEach(function(key){
                            console.log(key,obj[key]);
                            columns.push({"title":key,"key":key});
                        });
                        this.columnStandard=columns;
                        this.resultStandard = response.data;
                    }.bind(this)).catch(function (error) {
                        alert(error);
                    });

                }
            },
            generateOC(){
                if(this.tradeIdTop=="")
                {
                    alert("请输入顶层tradeId");
                }else if(this.sysCode03==""){
                    alert("请输入顶层sys_code");
                }else{

                    let sha= sha1.hex_sha1(this.tradeIdTop);
                    this.standardRowKey=sha.substring(sha.length-3)+this.sysCode03+sha;
                    this.axios({
                        method: 'get',
                        url: '/getRelationData',
                        async: false,
                        params: {
                            "tableName":'OC_ORDER',
                            "rowKey": this.standardRowKey
                        }
                    }).then(function (response) {
                        let obj=response.data[0];
                        let columns=[];
                        Object.keys(obj).forEach(function(key){
                            console.log(key,obj[key]);
                            columns.push({"title":key,"key":key});
                        });
                        this.columnOC=columns;
                        this.resultOC = response.data;
                    }.bind(this)).catch(function (error) {
                        alert(error);
                    });

                }

            }
        }
    };
</script>