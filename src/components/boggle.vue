<template>
  <div class="boggle">
    <h1>vue boggle</h1>

    <div class="timer">{{ minutes }}:{{ seconds | twoDigits }} ~ {{ counter }}</div>

    <div class="board">
      <div v-for="item in tiles" class="tile">
        <span v-if="counter > 0">{{ visibleSide(item) }}</span>
      </div>
    </div>

    <button @click="shuffleTiles(); resetTimer()" class="shake">SHAKE</button>

  </div>
</template>

<script>
  import _ from 'lodash'

  export default {
    name: 'boggle',
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
        // timer is done by default, letters hide at 0
        counter: 0,
        timerObj: null,
      }
    },
    computed: {
      seconds() {
        return Math.trunc(this.counter % 60);
      },
      minutes() {
        return Math.trunc(this.counter / 60);
      },
    },
    methods: {
      visibleSide(item) {
        return _.sample(item);
      },
      shuffleTiles() {
        this.tiles = _.shuffle(this.tiles);
      },
      resetTimer() {
        this.counter = 180;
        this.startCountdown();
      },
      startCountdown() {
        this.timerObj = window.setInterval(this.timerTick, 1000);
      },
      stopTimer() {
        window.clearInterval(this.timerObj);
      },
      timerTick(){
        if (this.counter > 0) {
          this.counter -= 1;
        } else {
          this.stopTimer();
          alert('timer done!');
        }
      },
    },
    filters: {
      twoDigits: function (value) {
        if (value.toString().length <= 1) {
          return '0' + value.toString();
        }
        return value.toString();
      }
    }
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

  /* time counter */
  .timer {
    margin: 25px;
    font-size: 25px;
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
