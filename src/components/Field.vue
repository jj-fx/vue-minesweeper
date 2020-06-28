<template>
    <div class="square" @click="selectField">
        {{ field }}
    </div>
</template>

<script>
    export default {
        name: "Field",
        methods: {
            selectField() {
                console.log(this.field);
            }
        },
        props: {
            field: {
                type: String,
                default: 'X',
            },
        },
    }
</script>

<style scoped>
    .square {
        background: #fff;
        border: 1px solid #999;
        float: left;
        font-size: 24px;
        font-weight: bold;
        line-height: 34px;
        height: 34px;
        margin-right: -1px;
        margin-top: -1px;
        padding: 0;
        text-align: center;
        width: 34px;
    }
</style>