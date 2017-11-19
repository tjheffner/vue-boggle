<template>
  <div class="boggle">
    <h1>boggle</h1>

    <div class="board">
      <div v-for="item in tiles" class="tile">
        <span v-if="showTiles">{{ visibleSide(item) }}</span>
      </div>
    </div>

    <div class="controls">
      <timer v-bind:startTime="this.roundStart"
             v-bind:roundLength="this.roundLength"
             v-bind:roundNumber="this.roundNumber"
             v-on:hide="hideTiles">
      </timer>

      <button @click="startGame" class="shake">SHAKE</button>
    </div>

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
        roundLength: 180,
        roundNumber: 0
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
      // acts on 'hide' event emitted from timer.vue
      hideTiles() {
        this.showTiles = false;
      }
    },
  }
</script>

<style lang="scss">
  @import './styles.scss';
</style>
