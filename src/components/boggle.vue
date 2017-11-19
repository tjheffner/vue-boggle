<template>
  <div class="boggle">
    <h1>vue boggle</h1>

    <timer v-bind:startTime="this.roundStart"
           v-bind:roundLength="this.roundLength"
           v-bind:roundNumber="this.roundNumber">
    </timer>

    <div class="board">
      <div v-for="item in tiles" class="tile">
        <span v-if="showTiles" v-on:hide="hideTiles">{{ visibleSide(item) }}</span>
      </div>
    </div>

    <button @click="startGame" class="shake">SHAKE</button>

  </div>
</template>

<script>
  import _ from 'lodash'
  import timer from './timer'

  export default {
    name: 'boggle',
    components: { timer },
    data() {
      return {
        // copied from a 1992 edition of boggle I found at goodwill
        tiles: [
          ['D', 'E', 'Y', 'R', 'L', 'V'],
          ['W', 'A', 'T', 'T', 'O', 'O'],
          ['D', 'R', 'L', 'X', 'I', 'E'],
          ['E', 'I', 'O', 'T', 'S', 'S'],
          ['B', 'B', 'O', 'A', 'J', 'O'],
          ['P', 'S', 'O', 'H', 'C', 'A'],
          ['G', 'A', 'E', 'A', 'E', 'N'],
          ['V', 'W', 'T', 'E', 'R', 'H'],
          ['N', 'R', 'L', 'Z', 'H', 'N'],
          ['F', 'S', 'P', 'F', 'K', 'A'],
          ['I', 'Qu', 'U', 'N', 'M', 'I'],
          ['G', 'H', 'E', 'E', 'W', 'N'],
          ['N', 'E', 'E', 'U', 'I', 'S'],
          ['D', 'S', 'T', 'I', 'T', 'Y'],
          ['L', 'R', 'T', 'Y', 'E', 'T'],
          ['C', 'M', 'U', 'T', 'O', 'I'],
        ],
        showTiles: false,
        roundStart: null,
        roundLength: 10,
        roundNumber: 1
      }
    },
    methods: {
      // randomize array of tiles
      shuffleTiles() {
        this.tiles = _.shuffle(this.tiles);
      },
      // pick random side from given tile
      visibleSide(item) {
        return _.sample(item);
      },
      startGame() {
        this.shuffleTiles();
        this.showTiles = true;
        this.roundStart = Math.trunc((Date.now()) / 1000);
        this.roundNumber++;
      },
      hideTiles() {
        this.showTiles = false;
      }
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


  /* overall container */
  .boggle {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 0 auto;
    font-family: sans-serif;
    font-weight: bold;
    height: 100%;
    background: mintcream;
  }

  /* wrapper around tiles */
  .board {
    background: dodgerblue;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    width: 550px;
    border-radius: 15px;
    border: 10px solid hotpink;
  }
  /* individual tile */
  .tile {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100px;
    width: 100px;
    margin: 15px;
    border-radius: 15px;
    background: mintcream;
    color: black;
    font-size: 60px;
  }

  /* reset button */
  .shake {
    margin: 50px;
    height: 100px;
    width: 25%;
    color: hotpink;
    background: dodgerblue;
    border: 4px solid hotpink;
    border-radius: 15px;
    font-size: 48px;
  }
</style>
