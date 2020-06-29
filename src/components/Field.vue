<template>
    <div :class="classObject" @click="changeState">
        {{ field }}
    </div>
</template>

<script>
    export default {
        name: "Field",
        data() {
            return {
                explored: false,
            }
        },
        props: {
            id: {
                type: Number,
                default: -1,
            },
            field: {
                type: String,
                default: 'X',
            },
        },
        methods: {
            changeState() {
                this.$emit("updateField", this.id);
                this.explored = true;
                if (this.field === 'X')
                    console.log('You lost!');
            }
        },
        computed: {
            classObject: function () {
                if (this.explored === false) {
                    return 'square unexplored';
                } else {
                    if (this.field === 'X')
                        return 'square mine';
                    else if (this.field === '0')
                        return 'square v0';
                    else if (this.field === '1')
                        return 'square v1';
                    else if (this.field === '2')
                        return 'square v2';
                    else if (this.field === '3')
                        return 'square v3';
                    else if (this.field === '4')
                        return 'square v4';
                    else if (this.field === '5')
                        return 'square v5';
                    else if (this.field === '6')
                        return 'square v6';
                    else if (this.field === '7')
                        return 'square v7';
                    else if (this.field === '8')
                        return 'square v8';
                    else
                        return 'square';
                }
            }
        },
    }
</script>

<style scoped>
    .square {
        background: #c9c9c9;
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

    .mine {
        background: #bf1414;
        color: rgba(0, 0, 0, 125);
    }

    .unexplored {
        background: #585858;
        color: rgba(255, 255, 255, 0);
    }

    .v0 {
        color: rgba(255, 255, 255, 0);
    }

    .v1 {
        color: #f8ff74;
    }

    .v2 {
        color: #74f7ff;
    }

    .v3 {
        color: #8a82ff;
    }

    .v4 {
        color: #3046ff;
    }

    .v5 {
        color: #15fd2e;
    }

    .v6 {
        color: #eefc07;
    }

    .v7 {
        color: #ff571a;
    }

    .v8 {
        color: #ff13af;
    }
</style>