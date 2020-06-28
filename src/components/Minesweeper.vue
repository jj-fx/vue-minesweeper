<template>
    <div class="minefield">
        <li class="column" v-for="col in columns" :key="col">
            <ul class="row" v-for="row in rows" :key="row">
                <Field />
            </ul>
        </li>
    </div>
</template>

<script>
    import Field from "@/components/Field";

    export default {
        name: "Minesweeper",
        components: {
            Field,
        },
        data() {
            return {
                name: this.name,
                rows: 7,
                columns: 7,
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

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
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
