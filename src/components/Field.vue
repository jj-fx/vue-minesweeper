<template>
    <img :src="textureObject" :class="classObject" @click="changeState">

</template>

<script>
    export default {
        name: "Field",
        props: {
            id: {
                type: Number,
                default: -1,
            },
            field: {
                type: String,
                default: 'X',
            },
            explored: {
                type: Boolean,
                default: false,
            },
            youLose: {
                type: Boolean,
                default: false,
            },
            youWin: {
                type: Boolean,
                default: false,
            },
            guess: {
                type: Boolean,
                default: false,
            },
        },
        methods: {
            changeState() {
                if (!this.youLose && !this.youWin) {
                    this.$emit("update", this.id);
                }
            },
        },
        computed: {
            textureObject() {
                let texture = 0;
                for (let i=1; i<=8; i++) {
                    if (this.field === i.toString())
                        texture = i.toString();
                }
                if (this.field === '0' || this.explored === false)
                    return require(`@/assets/null.png`);
                else if (this.field === 'X')
                    return require('@/assets/mine.png');
                else return require(`@/assets/number_${texture}.png`);

            },
            classObject() {
                let texture = '';
                for (let i=1; i<=8; i++) {
                    if (this.field === i.toString())
                        texture = `f${i}`;
                    else if (this.field === 'X')
                        texture = 'X';
                }
                // Game Over
                if (this.youLose && this.field === 'X')
                    texture = 'mine';
                if (!this.explored)
                    texture = 'unexplored'

                return `square ${texture}`;
            }
        },
    }
</script>

<style scoped>
    .square {
        height: 54px;
        width: 54px;
        //line-height: 44px;
        //background: #c9c9c9;
        border: 3px solid #32302e;
        border-radius: 5px;
        float: left;
        font-size: 24px;
        font-weight: bold;
        margin-right: -1px;
        margin-top: -1px;
        padding: 0;
        //text-align: center;
    }

    .mine {
        background: #bf1414;
        color: rgba(0, 0, 0, 125);
    }

    .unexplored {
        background-color: #181716;
        //color: rgba(0, 0, 0, 0);
    }

    .f0 {
        background-color: rgb(92, 101, 104);
        filter: brightness(1.25) saturate(0.5);
    }

    .f1 {
        background-color: rgb(227, 219, 164);
        filter: brightness(0.75) saturate(2);
    }

    .f2 {
        background-color: #66e4fc;
        filter: brightness(0.75) saturate(2);
    }

    .f3 {
        background-color: #9f99ff;
        filter: brightness(0.75) saturate(2);
    }

    .f4 {
        background-color: #97ff94;
        filter: brightness(0.75) saturate(2);
    }

    .f5 {
        background-color: #d32dde;
        filter: brightness(0.75) saturate(2);
    }

    .f6 {
        background-color: #08ffb1;
        filter: brightness(0.75) saturate(2);
    }

    .f7 {
        background-color: #ff571a;
        filter: brightness(0.75) saturate(2);
    }

    .f8 {
        background-color: #ff0000;
        filter: brightness(0.75) saturate(2);
    }
</style>