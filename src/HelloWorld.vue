<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      That's my boys!
    </p>
    <br>
    <input type="text"
           v-model="players[0].name">
    <strong> vs </strong>
    <input type="text"
           v-model="players[1].name">
    <br><br>
    <button @click="startGame">Start Game</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      players: [
        {
          name: 'A',
        },
        {
          name: 'B',
        },
      ],
    }
  },
  methods: {
    playersDice(player) {

    },
    startGame() {
      let tiles = [];
      let dices = [
        ['A','A','A','A','A'],
        ['A','A','A','A','A'],
        ['A','A',null,'B','B'],
        ['B','B','B','B','B'],
        ['B','B','B','B','B'],
      ];

      for (let x = 0; x < 5; x++)
      {
        tiles[x] = [];
        for (let y = 0; y < 5; y++)
          tiles[x][y] = {
            p: dices[x][y],
            s: dices[x][y] ? 1:0,
          };
      }

      this.$store.state.board = {
        tiles: tiles,
        players: {
          'A': {
            'name': this.players[0].name,
          },
          'B': {
            'name': this.players[1].name,
          },
        }
      };
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
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
a, button {
  color: #42b983;
}
button {
  border: solid;
  font-size: 20px;
}
</style>
