<template>
  <div id="app">
    <div class="header">
      <h1>Destiny 2: Forsaken Schedule</h1>
    </div>
    <Timeline />
  </div>
</template>

<script>
import Timeline from './components/Timeline.vue';

export default {
  name: 'app',
  components: {
    Timeline
  }
}
</script>

<style lang="scss">
  @import '~normalize-css/normalize.css';
  @import 'scss/imports';

  * {
    box-sizing: border-box;
  }

  html, body, #app {
    height: 100%;
  }

  html,
  body {
    font-size: 100%;
    margin: 0;
    padding: 0;
  }

  h1 {
    font-weight: 400;
    text-transform: uppercase;
  }

  .header {
    height: 100px;
    padding-top: 1px;
  }

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: $white;
    background: $dark;

    a {
      color: rgba($highlight, 0.8);

      &:hover {
        color: rgba($highlight, 1);
      }
    }
  }
</style>
