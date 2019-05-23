
<template>
    <div>
        <Table :columns="columns" :data="row.detail"></Table>
    </div>
</template>
<script>
    export default {
        props: {
            row: Object,
            columns:Array
        }
    };
</script>