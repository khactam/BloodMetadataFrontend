<template>
  <div id="app">
    <button v-for="area in areas" :key="area.id" @click="changeData(area)">{{area.name}}</button>
    <Display :selectedArea="selectedArea" :spec="spec" />
  </div>
</template>

<script>
import Display from './components/Display'

export default {
  name: 'App',
  components: {
    Display
  },
  data: () => ({
    spec: null,
    areas: [],
    selectedArea: {}
  }),
  async mounted () {
    /* eslint-disable */
    this.getArea().then((areas) => {
      this.areas = areas.map((area) => {
        return {
          name: area.get('name'),
          range: area.get('frequencies'),
          id: area.get('id')
        }
      })
    })
  },
  methods: {
    getArea () {
      return Parse.Cloud.run('getArea', {})
    },
    getSpec: async function (areaName) {
      return Parse.Cloud.run('getSpec', { area: areaName }).then(response => {
        return response;
      }).catch(err => {
        throw err
      })
    },
    changeData: async function (area) {
      this.selectedArea = area
      this.spec = await this.getSpec(area.name)
    }
  },
}
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
