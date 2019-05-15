<template>
    <div style="margin: 20px;">
        <div>
            <Row style="margin-bottom: 25px;">
                <Col span="12">消费组：
                    <Select v-model="menuId"  multiple style="width: 400px">
                        <Option v-for="item in menuList" :value="item.value" :key="item.value">{{ item.label }}</Option>
                    </Select>
                </Col>

            </Row>
            <Row style="margin-bottom: 25px;">
                <Col span="8">时间：
                    <DatePicker type="date" placeholder="Select date" style="width: 200px"></DatePicker>
                </Col>
                <Col span="8"><Button type="primary" shape="circle" icon="ios-search" @click="search()">搜索</Button></Col>
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


                    <div style="text-align: right;">
                        <Page :total="total" :page-size="pageInfo.pageSize" show-elevator show-total @on-change="e=>{pageSearch(e)}"></Page>
                    </div>


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
                groupId:null,
                /*用于查找的菜单id*/
                menuId:null,
                /*选择的数量*/
                count:null,
                /*选中的组数据*/
                groupId:null,
                /*新建modal的显示参数*/
                newModal:false,
                /*修改modal的显示参数*/
                modifyModal:false,
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
                    {
                        groupId: 'T0000_CB.DOMAIN1.ROUTER.ACCESS_0630_221',
                        topic: 18,
                        address: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]

                    },
                    {
                        groupId: 'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221',
                        topic: 25,
                        address: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]
                    }
                    ,
                    {
                        groupId: 'T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221',
                        topic: 25,
                        address: [{"partition":"0","lag":"10010"},{"partition":"1","lag":"10012"}]
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
                    }
                ],
                /*菜单列表*/
                menuList:[{
                    label:"T0000_CB.DOMAIN1.ROUTER.ACCESS_0630_221",value:"1"
                }, {
                        label:"T0000_CB.DOMAIN2.ROUTER.ACCESS_0630_221",value:"2"
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
            this.getLags({
                "pageInfo":this.pageInfo,
                'menuId':this.menuId
            });
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
            /*pageInfo实体初始化*/
            initPageInfo(){
                this.pageInfo.page = 0;
                this.pageInfo.pageSize = 10;
            },
            /*menu实体初始化*/
            initMenu(){
                this.menu.id = null;
                this.menu.name = null;
                this.menu.url = null;
                this.menu.parentId = null;
                this.menu.sort = null;
                this.menu.remark = null;
                this.menu.icon = null;
            },
            /*menuNew实体初始化*/
            initMenuNew(){
                this.menuNew.id = null;
                this.menuNew.name = null;
                this.menuNew.url = null;
                this.menuNew.parentId = null;
                this.menuNew.sort = null;
                this.menuNew.remark = null;
                this.menuNew.icon = null;
            },
            /*menuModify实体初始化*/
            initMenuModify(){
                this.menuModify.id = null;
                this.menuModify.name = null;
                this.menuModify.url = null;
                this.menuModify.parentId = null;
                this.menuModify.sort = null;
                this.menuModify.remark = null;
                this.menuModify.icon = null;
            },
            /*menuNew设置*/
            menuSet(e){
                this.menu.id = e.id;
                this.menu.name = e.name;
                this.menu.url = e.url;
                this.menu.parentId = e.parentId;
                this.menu.sort = e.sort;
                this.menu.remark = e.remark;
                this.menu.icon = e.icon;
            },
            /*menuNew设置*/
            menuNewSet(e){
                this.menuNew.id = e.id;
                this.menuNew.name = e.name;
                this.menuNew.url = e.url;
                this.menuNew.parentId = e.parentId;
                this.menuNew.sort = e.sort;
                this.menuNew.remark = e.remark;
                this.menuNew.icon = e.icon;
            },
            /*menuModify设置*/
            menuModifySet(e){
                this.menuModify.id = e.id;
                this.menuModify.name = e.name;
                this.menuModify.url = e.url;
                this.menuModify.parentId = e.parentId+'';
                this.menuModify.sort = e.sort+'';
                this.menuModify.remark = e.remark;
                this.menuModify.icon = e.icon;
            },
            /*得到表数据*/
            getLags(e) {
                this.axios({
                    method: 'get',
                    url: '/getLags',
                    params: {
                        'page':e.pageInfo.page,
                        'pageSize':e.pageInfo.pageSize,
                        'menuId':e.menuId
                    }
                }).then(function (response) {
                    this.data1=response.data.data;
                    this.total=response.data.totalCount;
                }.bind(this)).catch(function (error) {
                    alert(error);
                });
            },
            /*搜索按钮点击事件*/
            search(){
                this.initPageInfo();
                this.getTable({
                    "pageInfo":this.pageInfo,
                    'menuId':this.menuId
                });
            },
            /*分页点击事件*/
            pageSearch(e){
                this.pageInfo.page = e-1;
                this.getTable({
                    "pageInfo":this.pageInfo,
                    'menuId':this.menuId
                });
            },
            /*新建点击触发事件*/
            openNewModal(){
                this.newModal = true;
                this.initMenuNew();
                this.data1.sort();
                this.count = 0;
                this.groupId = null;
            },
            /*新建modal的newOk点击事件*/
            newOk (menuNew) {
                this.$refs[menuNew].validate((valid) => {
                    if (valid) {
                        this.initMenu();
                        this.menuSet(this.menuNew);
                        this.axios({
                            method: 'post',
                            url: '/menus/menu',
                            data: this.menu
                        }).then(function (response) {
                            this.initMenuNew();
                            this.getTable({
                                "pageInfo":this.pageInfo,
                                'menuId':this.menuId
                            });
                            this.$Message.info('新建成功');
                        }.bind(this)).catch(function (error) {
                            alert(error);
                        });
                        this.newModal = false;
                    }else {
                        setTimeout(() => {
                            this.loading = false;
                            this.$nextTick(() => {
                                this.loading = true;
                            });
                        }, 1000);
                    }
                })
            },
            /*点击修改时判断是否只选择一个*/
            openModifyModal(){
                if(this.count > 1 || this.count < 1){
                    this.modifyModal= false;
                    this.$Message.warning('请至少选择一项(且只能选择一项)');
                }else{
                    this.modifyModal = true;
                }
            },
            /*修改modal的modifyOk点击事件*/
            modifyOk (menuModify) {
                this.$refs[menuModify].validate((valid) => {
                    if (valid) {
                        this.initMenu();
                        this.menuSet(this.menuModify);
                        this.axios({
                            method: 'put',
                            url: '/menus/'+this.menu.id,
                            data: this.menu
                        }).then(function (response) {
                            this.initMenuNew();
                            this.getTable({
                                "pageInfo":this.pageInfo,
                                'menuId':this.menuId
                            });
                            this.$Message.info('修改成功');
                        }.bind(this)).catch(function (error) {
                            alert(error);
                        });
                        this.modifyModal = false;
                    }else {
                        this.$Message.error('表单验证失败!');
                        setTimeout(() => {
                            this.loading = false;
                            this.$nextTick(() => {
                                this.loading = true;
                            });
                        }, 1000);
                    }
                })
            },
            /*modal的cancel点击事件*/
            cancel () {
                this.$Message.info('点击了取消');
            },
            /*table选择后触发事件*/
            change(e){
                if(e.length==1){
                    this.menuModifySet(e['0']);
                }
                this.setGroupId(e);
            },
            /*通过选中的行设置groupId的值*/
            setGroupId(e){
                this.groupId=[];
                this.count=e.length;
                for (var i = 0; i <= e.length - 1; i++) {
                    this.groupId.push(e[i].id);
                }
            },
            /*删除table中选中的数据*/
            del(){
                if(this.groupId!=null && this.groupId!=""){
                    this.axios({
                        method: 'delete',
                        url: '/menus',
                        data: this.groupId
                    }).then(function (response) {
                        this.getTable({
                            "pageInfo":this.pageInfo,
                            'menuId':this.menuId
                        });
                        this.groupId=null;
                        this.count=0;
                        this.$Message.info('删除成功');
                    }.bind(this)).catch(function (error) {
                        alert(error);
                    });
                }
            },
            /*表格中双击事件*/
            dblclick(e){
                this.menuModifySet(e);
                this.modifyModal = true;
                this.data1.sort();
            }
        }
    }
</script>