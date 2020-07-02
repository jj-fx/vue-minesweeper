<template>
    <div class="game-container">
        <div class="game-area">
            <div class="reset-container">
                <input id="mine-counter" class="mine-counter" disabled="true" type="number" :value="mineCounter" style="width: 60px">
                <button class="reset" @click="resetMinefield(rows, maxMineCount)">RESET</button>
                <input id="game-size" class="reset" type="number" :value="rows" style="width: 60px" @change="setMinefieldSize">
            </div>
            <div class="minefield">
                <ul class="column" v-for="col in columns" :key="col">
                    <li class="row" v-for="row in rows" :key="row">
                        <Field
                                :field="mineFields[(row + (col - 1) * rows) - 1].toString()"
                                :id="(row + (col - 1) * rows) - 1"
                                :explored="exploredFields[(row + (col - 1) * rows) - 1]"
                                :youLose="gameOver"
                                :youWin="gameWin"
                                @update="exploreField"
                        />
                    </li>
                </ul>
            </div>
        </div>
        <div>
            <h1 :class="classObject">
                {{ message }}
            </h1>
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
            this.resetMinefield(this.rows, this.maxMineCount);
            this.fields = {
                value:[...this.mineFields],
                explored:[...this.exploredFields]
            };
        },
        data() {
            return {
                rows: 5,
                columns: 5,
                difficulty: 15,
                maxMineCount: 3,
                mineCounter: 0,
                mineFields: [],
                exploredFields: [],
                message: 'Just click on cells...',
                gameOver: false,
                gameWin: false,
                fields: [],
            }
        },
        methods: {
            setMinefieldSize() {
                const size = parseInt(document.getElementById("game-size").value);
                this.maxMineCount = Math.floor(Math.pow(size, 2) / this.difficulty);
                this.rows = size; this.columns = size;
                this.resetMinefield(this.rows, this.maxMineCount);
            },
            exploreField(field_index) {
                // Game Over
                if (this.mineFields[field_index] === 'X') {
                    this.message = 'GAME OVER!';
                    this.gameOver = true;
                }

                // Set as explored
                this.$set(this.exploredFields, field_index, true);

                // Win condition:
                const leftToExplore = this.exploredFields.filter(item => { return item === false; });
                if (this.maxMineCount === leftToExplore.length) {
                    this.message = '!!! You Win !!!';
                    this.gameWin = true;
                }
                // explore empty fields
                if (this.mineFields[field_index] === 0) {
                    const neighbors = this.getCloseFields(field_index, true);
                    neighbors.forEach((item) => {
                        this.exploreEmpty(item);
                    });
                }
                // get the expanded
                const toExpand = new Set();
                this.exploredFields.forEach((item, index) => {
                    if (this.mineFields[index] === 0 && item === true) {
                        const neighbors = this.getCloseFields(index, true);
                        neighbors.forEach((item) => {
                            toExpand.add(item);
                        });
                    }
                });
                // unhide expansion
                toExpand.forEach((item) => {
                    if (this.exploredFields[item] === false) {
                        this.$set(this.exploredFields, item, true);
                    }
                });
            },
            resetMinefield(fieldSize, mineCount) {
                this.gameWin = false;
                this.mineCounter = this.maxMineCount;
                this.message = 'Just click on cells...';
                this.gameOver = false;
                // Init empty array
                this.mineFields = Array(Math.pow(fieldSize, 2)).fill('');
                this.exploredFields = Array(Math.pow(fieldSize, 2)).fill(false);

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
                    count += this.isThisAMine(row + 1, col, item, 'X');
                    // S
                    count += this.isThisAMine(row - 1, col, item, 'X');
                    // E
                    count += this.isThisAMine(row, col - 1, item, 'X');
                    // W
                    count += this.isThisAMine(row, col + 1, item, 'X');
                    // N-E
                    count += this.isThisAMine(row + 1, col - 1, item, 'X');
                    // N-W
                    count += this.isThisAMine(row + 1, col + 1, item, 'X');
                    // S-E
                    count += this.isThisAMine(row - 1, col - 1, item, 'X');
                    // S-W
                    count += this.isThisAMine(row - 1, col + 1, item, 'X');

                    if (item !== 'X') {
                        this.mineFields[index] = count;
                    }
                });
            },
            // is the field (row, col) inside the minefield?
            isValid(row, col) {
                return (row >= 0) && (row < this.rows) && (col >= 0) && (col < this.columns);
            },
            // is the item a value (value = mine)?
            isThisAMine(row, col, item, value) {
                if (item !== value) {
                    if (this.isValid(row, col) === true) {
                        const field = col * this.rows + row;
                        if (this.mineFields[field] === value)
                            return 1;
                        else return 0;
                    } else return 0;
                } else return 0;
                // too many returns here. Clean this shit up later dude!
            },
            //
            deconstruct(index) {
                const row = index % this.rows;
                const col = Math.floor(index / this.columns);
                return [row, col];
            },
            //
            getCloseFields(index, corners) {
                let array = [];
                const pos = this.deconstruct(index);
                const row = pos[0];
                const col = pos[1];
                // N
                if (this.isValid(row - 1, col) === true) {
                    array.push(index - 1);
                }
                // // S
                if (this.isValid(row + 1, col) === true) {
                    array.push(index + 1);
                }
                // E
                if (this.isValid(row, col + 1) === true) {
                    array.push(index + this.rows);
                }
                // W
                if (this.isValid(row, col - 1) === true) {
                    array.push(index - this.rows);
                }
                if (corners === true) {
                    // N - E
                    if (this.isValid(row - 1, col + 1) === true) {
                        array.push(index + this.rows - 1);
                    }
                    // // N - W
                    if (this.isValid(row - 1, col - 1) === true) {
                        array.push(index - this.rows - 1);
                    }
                    // // S - E
                    if (this.isValid(row + 1, col + 1) === true) {
                        array.push(index + this.rows + 1);
                    }
                    // // S - W
                    if (this.isValid(row + 1, col - 1) === true) {
                        array.push(index - this.rows + 1);
                    }
                }
                return array;
            },
            //
            exploreEmpty(field_index) {
                if (this.mineFields[field_index] === 0 && this.exploredFields[field_index] === false) {
                    // this item is empty
                    this.$set(this.exploredFields, field_index, true);
                    const neighbors = this.getCloseFields(field_index, true);
                    neighbors.forEach((item) => {
                        this.exploreEmpty(item);
                    });
                }
            }
        },
        computed: {
            classObject: function() {
                if (this.message === 'GAME OVER!') {
                    return 'game-over bottom';
                } else if (this.message === '!!! You Win !!!') {
                    return 'win bottom';
                } else return 'default bottom';
            }
        }
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

    .mine-counter {
        -webkit-appearance: none;
        margin: 0;
        font-size: 30px;
    }

    .game-container {
        //height: 900px;
    }

    .reset-container {
        margin-bottom: 20px;
        //width: 400px;
        display: flex;
        justify-content: space-evenly;
    }

    .reset {
        font-size: 30px;
        //width: 100px;
    }

    .game-area {
        //display: flex;
        //flex-direction: column;
        //justify-content: space-around;
        //height: auto;
        //padding: 2px;
    }

    .minefield {
        display: flex;
        justify-content: center;
        background-color: #7d7d84;
        //border-radius: 5px;
    }

    .column {
        //background-color: #3046ff;
    }

    .bottom {

    }

    .game-over {
        color: red;
    }

    .win {
        color: greenyellow;
    }

    .default {
        color: white;
    }
</style>
