<template>
  <div class="timer">
    <p v-if="startTime" v-show="timeLeft > 0">{{ timeLeft | parseTime }}</p>
  </div>
</template>

<script>
  export default {
    name: 'timer',
    props: [ 'startTime', 'roundLength' ],
    data() {
      return {
        now: null,
        interval: null,
        timeLeft: null
      }
    },
    methods: {
      updateTimeLeft: function() {
        this.interval = setInterval(function() {
          this.now = Math.trunc((Date.now()) / 1000);
        }.bind(this), 1000);
      },
    },
    watch: {
      now: function() {
        this.timeLeft = ((this.startTime + this.roundLength) - this.now);
      }
    },
    mounted() {
      this.updateTimeLeft()
    },
    beforeDestroy() {
      clearInterval(this.interval);
    },
    filters: {
      parseTime: function(value) {
        return value;
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>