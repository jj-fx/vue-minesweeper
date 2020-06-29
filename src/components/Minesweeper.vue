<template>
    <div>
        <div class="game-area">
            <div>
                <button @click="resetMinefield(rows, mineCount), resetFields">RESET</button>
            </div>
            <div class="minefield">
                <ul class="column" v-for="col in columns" :key="col">
                    <li class="row" v-for="row in rows" :key="row">
                        <Field
                                :field="mineFields[(row + (col - 1) * rows) - 1].toString()"
                                :id="(row + (col - 1) * rows) - 1"
                                @updateField="exploreField"
                        />
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
            this.resetMinefield(this.rows, this.mineCount);
        },
        data() {
            return {
                rows: 7,
                columns: 7,
                mineCount: 12,
                mineFields: [],
                exploredFields: [],
            }
        },
        methods: {
            resetFields() {
                console.log('reset');
            },
            exploreField(index) {
                this.exploredFields[index] = true;
                //console.log(index);
                return this.exploredFields;
            },
            resetMinefield(fieldSize, mineCount) {
                // Init empty array
                this.mineFields = Array(Math.pow(fieldSize, 2)).fill('');
                this.exploredFields = Array(Math.pow(fieldSize, 2)).fill(false);
                this.$emit("resetField");

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

                this.mineFields.forEach((item, index) => {
                    let count = 0;
                    // get columns
                    const col = Math.floor(index / this.rows);
                    // get rows
                    const row = index % this.rows;

                    // N
                    count += this.inspectField(row + 1, col, item, 'X');

                    // S
                    count += this.inspectField(row - 1, col, item, 'X');

                    // E
                    count += this.inspectField(row, col - 1, item, 'X');

                    // W
                    count += this.inspectField(row, col + 1, item, 'X');

                    // N-E
                    count += this.inspectField(row + 1, col - 1, item, 'X');

                    // N-W
                    count += this.inspectField(row + 1, col + 1, item, 'X');

                    // S-E
                    count += this.inspectField(row - 1, col - 1, item, 'X');

                    // S-W
                    count += this.inspectField(row - 1, col + 1, item, 'X');

                    if (item !== 'X') {
                        this.mineFields[index] = count;
                    }
                });
                //return this.mineFields;
            },
            isValid(row, col) {
                return (row >= 0) && (row < this.rows) && (col >= 0) && (col < this.columns);
            },
            inspectField(row, col, item, value) {
                if (item !== value) {
                    if (this.isValid(row, col) === true) {
                        const field = col * this.rows + row;
                        if (this.mineFields[field] === value)
                            return 1;
                        else return 0;
                    } else return 0;
                } else return 0;
            }
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
        /*display: inline-block;*/
        margin: 0;
        width: 35px;
    }
</style>
