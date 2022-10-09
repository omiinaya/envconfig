<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
  },
  methods: {
    async print(a) {
      window.webContents.send('LOG_REQUEST', a);
    },
    async getNodeList() {
      const url = 'https://nodejs.org/dist/';
      const req = await fetch(url);
      const string = await req.text();
      const lines = string.split('\n');
      const versions = lines.filter((line) => line.includes('<a href="v'));
      const parsed = [];
      versions.forEach((line) => parsed.push(line.split('href="v').pop().split('/">')[0]));
      parsed.sort((a, b) => {
        const num1 = Number(
          a
            .split('.')
            .map((num) => `000${num}`.slice(-3))
            .join(''),
        );
        const num2 = Number(
          b
            .split('.')
            .map((num) => `000${num}`.slice(-3))
            .join(''),
        );
        return num1 - num2;
      });
      const results = parsed.reverse().slice(0, 100);
      this.print(results);
    },
  },
  created() {
    this.getNodeList();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
