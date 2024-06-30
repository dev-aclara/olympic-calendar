<template>
  <div class="header-container">
    <OlympicGamesIcon
      class="olympic-games-icon"
    />
    <span>Calendário Jogos Olímpicos</span>
  </div>
</template>

<script lang="ts">
import OlympicGamesIcon from './icons/OlympicGamesIcon.vue';

export default {
  name: 'MyComponent',
  components: {
    OlympicGamesIcon,
  },
};
</script>

<style scoped lang="scss">
.header-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
  background-color: #f2f9ff;
  border-radius: 20px;

  span {
    font-size: 24px;
    font-weight: bold;
    margin-top: 10px;
    margin-bottom: 20px;
  }
}

.container > * {
  margin: 0 auto;
}
</style>
