<template>
  <div class="stats">
    <p class="round">round: <span>{{ roundNumber }}</span></p>
    <p class="timer"
       v-if="startTime"
       v-bind:class="{ red: timeWarning }"> {{ timeLeft | parseTime }}</p>
  </div>
</template>

<script>
  export default {
    name: 'timer',
    props: [ 'startTime', 'roundLength', 'roundNumber' ],
    data() {
      return {
        now: null,
        interval: null,
        timeLeft: 0,
        timeWarning: false
      }
    },
    mounted() {
      // start the timer once data is present + mounted
      this.currentTime()
    },
    methods: {
      // set the timer interval to 1s, update current time each tick
      currentTime: function() {
        this.interval = setInterval(function() {
          this.now = Math.trunc((Date.now()) / 1000);
        }.bind(this), 1000);
      },
    },
    watch: {
      // watch current time changes, update time left to match
      now: function() {
        this.timeLeft = ((this.startTime + this.roundLength) - this.now);
        this.timeWarning = false;

        // make the time left red
        if (this.timeLeft <= 30) {
          this.timeWarning = true;
        }

        // hide the tiles if the round is over
        if (this.timeLeft <= 0) {
          this.timeLeft = 0;
          this.$emit('hide', false);
        }
      }
    },
    beforeDestroy() {
      clearInterval(this.interval);
    },
    filters: {
      // format timeLeft to 00:00
      parseTime: function(value) {

        let minutes = Math.trunc(value / 60);
        let seconds = value % 60;

        if (minutes < 10) {
          minutes = '0' + minutes;
        }

        if (seconds < 10) {
          seconds = '0' + seconds;
        }

        const newValue = minutes + ':' + seconds;
        return newValue;
      }
    }
  }
</script>
