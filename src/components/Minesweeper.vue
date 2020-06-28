<template>
    <div>
        <div class="game-area" @click="plantMines(rows, mineCount)">
            <div>
                <button>RESET</button>
            </div>
            <div class="minefield">
                <ul class="column" v-for="col in columns" :key="col">
                    <li class="row" v-for="row in rows" :key="row">
                        <Field :field="mineFields[(row + (col - 1) * rows) - 1].toString()"/>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    import Field from "@/components/Field";

    export default {
        name: "Minesweeper",
        components: {
            Field,
        },
        created() {
            this.plantMines(this.rows, this.mineCount);
        },
        data() {
            return {
                rows: 7,
                columns: 7,
                mineCount: 15,
                mineFields: [],
            }
        },
        methods: {
            plantMines(fieldSize, mineCount) {
                // Init empty array
                this.mineFields = Array(Math.pow(fieldSize, 2)).fill('');

                for (let i = 0; i < mineCount; i++) {
                    let plantMine = false;
                    do { // repeat so we hit the mineCount
                        const randomField = Math.floor(Math.random() * this.mineFields.length);
                        if (this.mineFields[randomField] === '') {
                            this.mineFields[randomField] = 'X';
                            plantMine = false;
                        } else plantMine = true;
                    } while (plantMine)
                }
            },
        },
    }
</script>

<style scoped>
    h3 {
        margin: 40px 0 0;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    a {
        color: #42b983;
    }

    .game-area {
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        height: 400px;
    }

    .minefield {
        display: flex;
        justify-content: center;
    }

    .column {
        display: inline-block;
        margin: 0;
    }
</style>
