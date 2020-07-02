<template>
    <img :src="textureObject" :class="classObject" @click="changeState">

</template>

<script>
    export default {
        name: "Field",
        data() {
            return {
                // result: '',
                // delay: 700,
                // clicks: 0,
                // timer: null
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
            explored: {
                type: Boolean,
                default: false,
            },
            lost: {
                type: Boolean,
                default: false,
            },
            guess: {
                type: Boolean,
                default: false,
            },
        },
        methods: {
            makeGuess() {
                //this.$emit("guess", this.id);
                //console.log('double click');
            },
            changeState() {
                //console.log(this.result);
                if (this.lost === false) {
                    this.$emit("update", this.id);
                }
            },
        },
        computed: {
            textureObject: function() {
                let num = 0;
                for (let i=1; i<=8; i++) {
                    if (this.field === i.toString()) {
                        num = i.toString();
                    }
                }
                if (this.field === '0' || this.explored === false) {
                    return require(`@/assets/null.png`);
                } else if (this.field === 'X') {
                    return require('@/assets/mine.png');
                } else return require(`@/assets/number_${num}.png`);

            },
            classObject: function() {
                // Guess
                if (this.guess === true) {
                    return 'square guess';
                }

                // Game Over
                if (this.lost === true && this.field === 'X') {
                    return 'square mine';
                }

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

    .guess {
        background: #ffa945;
        color: rgba(0, 0, 0, 0);
    }

    .mine {
        background: #bf1414;
        color: rgba(0, 0, 0, 125);
    }

    .unexplored {
        background-color: #181716;
        //color: rgba(0, 0, 0, 0);
    }

    .v0 {
        color: rgba(255, 255, 255, 0);
    }

    .v1 {
        background-color: rgb(227, 219, 164);
        filter: brightness(0.75) saturate(2);
    }

    .v2 {
        background-color: #66e4fc;
        filter: brightness(0.75) saturate(2);
    }

    .v3 {
        background-color: #9f99ff;
        filter: brightness(0.75) saturate(2);
    }

    .v4 {
        background-color: #97ff94;
        filter: brightness(0.75) saturate(2);
    }

    .v5 {
        background-color: #d32dde;
        filter: brightness(0.75) saturate(2);
    }

    .v6 {
        background-color: #08ffb1;
        filter: brightness(0.75) saturate(2);
    }

    .v7 {
        background-color: #ff571a;
        filter: brightness(0.75) saturate(2);
    }

    .v8 {
        background-color: #ff0000;
        filter: brightness(0.75) saturate(2);
    }
</style>