<template>
  <div id="app">
    <boggle/>
  </div>
</template>

<script>
import boggle from './components/boggle'
import timer from './components/timer'

export default {
  name: 'app',
  components: {
    boggle,
    timer
  }
}
</script>