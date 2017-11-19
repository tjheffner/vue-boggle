<template>
  <div class="timer">
    <p>round: {{ roundNumber }}</p>
    <p>{{ timeLeft | parseTime }}</p>
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

        if (this.timeLeft < 0) {
          this.timeLeft = 0;
          this.$emit('hide');
        }
      }
    },
    beforeDestroy() {
      clearInterval(this.interval);
    },
    filters: {
      // format seconds to 00:00
      parseTime: function(value) {
        return value;
      }
    }
  }
</script>



<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>