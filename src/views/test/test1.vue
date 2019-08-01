
<template>


    <div >

        <Button  @click="test()">测试redis</Button>
    </div>
</template>
<script>
    export default {
        data(){
            return {

            }
        },
        methods: {
            test(){
                this.axios({
                    method: 'get',
                    url: '/testRedis',
                    async:false,
                    params: {
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
    };
</script>