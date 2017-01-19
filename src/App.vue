<template>
  <div id="app">
      <h1>Vue Number Input</h1>
      <h2>Value: {{ number }}</h2>
      <div class="form-group">
        <number-input
          class="form-control input-lg text-center"
          v-model="number"
          @empty="empty"
          autofocus
          ref="component"
          :min="100"
          :max="130"
        >
        </number-input>
      </div>
      <div v-for="n in emptied" class="alert alert-info">{{ n }}. Input has been erased.</div>
  </div>
</template>

<script>
import NumberInput from './components/NumberInput'

export default {
  name: 'app',
  data() {
    return {
      number: '123',
      emptied: 0,
    }
  },
  methods: {
    empty() {
      this.emptied++
    },
  },
  components: {
    NumberInput
  },
  mounted () {
     this.$refs.component.select()
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
