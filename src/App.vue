<template>
  <div id="app">
		<vTable />
      <footerMain />
  </div>
</template>

<script>
// import {mapActions, mapGetters} from 'vuex'
import vTable from './components/table/v-table'
import footerMain from './components/table/footer'

export default {
  name: 'App',
  components: {
		vTable,
        footerMain
  }
}
</script>

<style>
    body{
        margin: 0;
    }
    #app {
      font-family: Avenir, Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      /*text-align: center;*/
      color: #2c3e50;
      margin-top: 60px;
    }
</style>
