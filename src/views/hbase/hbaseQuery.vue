
<template>
    <div  style="margin: 20px;">
         表名：ORDER_RELATION_TABLE
        <div>

            order_line_id：
            <Input  v-model="order_line_id" type="text" placeholder="请输入order_line_id" style="width: 200px;margin-right: 50px"></Input>
            sys_code:
            <Input  v-model="sys_code" type="text" placeholder="请输入sys_code" style="width: 200px;margin-right: 50px"></Input>


        </div>
        <div style="margin: 20px">
            relationRowKey:
            <Input  v-model="rowKey"  type="text" placeholder="" style="width: 400px;margin-right: 50px"></Input>
            <Button type="primary" shape="circle"  @click="generate()">generate</Button>
        </div>
        <div >
            <div style="padding: 10px 0;">
                <template>
                    <Table :columns="column" :data="result"></Table>
                </template>

            </div>
        </div>
        <Divider />
   <!--     <div>

            order_line_id：
            <Input  v-model="order_line_id" type="text" placeholder="请输入order_line_id" style="width: 200px;margin-right: 50px"></Input>
            sys_code:
            <Input  v-model="sys_code" type="text" placeholder="请输入sys_code" style="width: 200px;margin-right: 50px"></Input>
        </div>-->
    </div>

</template>
<!--
<script src="../../libs/sha1.js"></script>
-->


<script>
import sha1 from "../../libs/sha1.js";
export default {

        data(){
            return {
                order_line_id:null,
                sys_code:null,
                rowKey:null,
                result:[],
                column:[]

            }
        },
        methods: {
            generate(){
                if(this.order_line_id=="")
                {
                    alert("请输入order_line_id");
                }else if(this.sys_code==""){
                    alert("请输入sys_code");
                }else{

               let sha= sha1.hex_sha1(this.order_line_id);
               this.rowKey=sha.substring(sha.length-3)+this.sys_code+sha;
                    this.axios({
                        method: 'get',
                        url: '/getRelationData',
                        async: false,
                        params: {
                            "rowKey": this.rowKey
                        }
                    }).then(function (response) {
                        // alert("result"+response.data);

                        let obj=response.data[0];
                        let columns=[];
                        Object.keys(obj).forEach(function(key){

                            console.log(key,obj[key]);
                            columns.push({"title":key,"key":key});
                          //  columns.push()


                        });
                        this.column=columns;
                        this.result = response.data;
                       // alert(this.result);
                        /*this.data1=response.data.data;
                    this.total=response.data.totalCount;*/
                    }.bind(this)).catch(function (error) {
                        alert(error);
                    });

                }

            }
        }
    };
</script>