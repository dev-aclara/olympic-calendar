<template>
  <div id="app">
    <HomeHeader />
    <MatchList />
  </div>
</template>

<script lang="ts">
  import { defineComponent } from 'vue';
  import HomeHeader from './components/HomeHeader.vue';
  import MatchList from './components/match/MatchList.vue';

  export default defineComponent({
    name: 'App',
    components: {
      HomeHeader,
      MatchList
    },
  });
</script>

<style scoped lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: 20px;
}
</style>
