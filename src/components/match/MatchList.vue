<template>
  <el-tabs
    v-model="activeName"
    class="demo-tabs"
    @tab-click="handleClick"
  >
    <el-tab-pane
      label="Futebol"
      name="football"
    >
      <template #default>
        <div class="match-list">
          <MatchCard v-for="match in footballMatches" :key="match.id" :match="match" />
        </div>
      </template>
    </el-tab-pane>
    <el-tab-pane
      label="Vôlei"
      name="volleyball"
    >
      <template #default>
        <div class="match-list">
          <span>
            Em breve, traremos os detalhes dos jogos de vôlei. Estamos preparando tudo para trazer as informações mais recentes sobre as partidas. Fique de olho!
          </span>
        </div>
      </template>
    </el-tab-pane>
  </el-tabs>
</template>

<script lang="ts">
  import MatchCard from './MatchCard.vue';
  import { Match } from '../../interfaces/match';
  import { defineComponent, ref, onMounted } from 'vue';
  import { matchData } from '../../api/mocks/match-data';

  export default defineComponent({
    name: 'TabsWithMatchList',
    components: {
      MatchCard,
    },
    setup() {
      const footballMatches = ref<Match[]>([]);
      const activeName = ref('football');

      onMounted(() => {
        setTimeout(() => {
          footballMatches.value = matchData.filter(match => match.sport === 'Futebol');
        }, 1000);
      });

      const handleClick = (tab: any, event: Event) => {
        console.log(tab, event);
        activeName.value = tab.name;
      };

      return {
        activeName,
        footballMatches,
        handleClick,
      };
    },
  });
</script>

<style scoped lang="scss">
  .demo-tabs > .el-tabs__content {
    padding: 32px;
    color: #6b778c;
    font-size: 32px;
    font-weight: 600;
  }

  .match-list {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    padding: 20px;
  }

  .match-list > * {
    width: 100%;
    max-width: 320px;
    margin-bottom: 20px;
  }
</style>
