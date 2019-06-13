<template>
    <div>
        <Table :columns="columns" :data="row.detail">
          <template slot-scope="{ row, index }" slot="action">
            </template>
        </Table>
    </div>
</template>
<script>
    export default {
        modal:false,
        offset:"",
        props: {
            row: Object,
            columns:Array
        },
        components:{
            //form:"topic:${this.row.topic}<br>groupId:${this.row.groupId}<br>offset：<input v-model=\"offset\"></input>"
        },
        methods: {
            show (index) {
                 let self=this;

                this.$Modal.confirm({
                    title: '请确认是否调整偏移量',
                    content: `topic:${this.row.topic}<br>groupId:${this.row.groupId}<br>offset：<input v-model="offset"></input>`,
                    render: (h) => {
                        return h("input", {
                            props: {

                            }
                        })
                    },
                    onOk(){
                            alert(self.row.topic+""+self.row.groupId+""+this.offset);
                        }

                })
            },

            }

    };
</script>