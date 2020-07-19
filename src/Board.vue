<template>
    <div style="width:40%;display:table;margin:auto;">
        <hr>
        <h1 class="active" v-if="has_won">Winner!!!</h1>
        <table class="table">
            <tr v-for="(row, x) in $store.state.board.tiles">
                <td v-for="(tile, y) in row" class="row" @click="move(x, y)">
                    <span v-bind:class="{
                        'l-1': tile.s === 1,
                        'l-2': tile.s > 1,
                        'p-a': tile.p === 'A',
                        'p-b': tile.p === 'B',
                    }"></span>
                </td>
            </tr>
        </table>
    </div>
</template>

<script>
    export default {
        name: "Board",
        data() {
            return {
                empty_x_y: [],
                final_board: [1,2,3,4,5,6,7,8,'*'],
                has_won: false,
            }
        },
        created() {
            // for (let x in this.$store.state.board.tiles)
            //     for (let y in this.$store.state.board.tiles[x])
            //         if (this.$store.state.board.tiles[x][y] === '*')
            //             this.empty_x_y = [x*1, y*1];
        },
        methods: {
            movable(tile_x, tile_y) {

                let diff = 0;

                if (this.empty_x_y[0] === tile_x)
                    diff = this.empty_x_y[1] - tile_y;

                else if (this.empty_x_y[1] === tile_y)
                    diff = this.empty_x_y[0] - tile_x;

                return diff === 1 || diff === -1;
            },
            winner() {
                let counter = 0;
                for (let x in this.$store.state.board.tiles)
                    for (let y in this.$store.state.board.tiles[x])
                    {
                        if (this.$store.state.board.tiles[x][y] !== this.final_board[counter])
                            return false;
                        counter += 1;
                    }
                return true;
            },
            move(tile_x, tile_y) {

                if (this.movable(tile_x, tile_y))
                {
                    this.$store.state.board.tiles[this.empty_x_y[0]][this.empty_x_y[1]] = this.$store.state.board.tiles[tile_x][tile_y];
                    this.$store.state.board.tiles[tile_x][tile_y] = '*';
                    this.empty_x_y = [tile_x, tile_y];
                    this.has_won = this.winner();
                    this.$forceUpdate();
                }
            }
        },
    }
</script>

<style scoped>
    .table {
        width: 100%;
        background-color: #2196F3;
    }
    .row {
        border: 1px solid rgba(0, 0, 0, 0.8);
        padding: 5%;
        font-size: 50px;
        font-weight: bold;
        color: black;
        text-align: center;
        background-color: white;
    }
    span.p-a {
        background-color: red;
    }
    span.p-b {
        background-color: blue;
    }
    span.l-1, span.l-2 {
        height: 25px;
        width: 25px;
        border-radius: 50%;
        display: inline-block;
    }
    span.l-1 {
        border: 2px solid black;
    }
    span.l-2 {
        border: 6px solid black;
    }
</style>