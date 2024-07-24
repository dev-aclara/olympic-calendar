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
          <FootballCard v-for="match in footballMatches" :key="match.id" :match="match" />
        </div>
      </template>
    </el-tab-pane>

    <el-tab-pane
      label="Vôlei"
      name="volleyball"
    >
      <template #default>
        <div class="match-list">
          <VolleyCard v-for="match in volleyballMatches" :key="match.id" :match="match" />
        </div>
      </template>
    </el-tab-pane>

    <el-tab-pane
      label="Skate"
      name="skate"
    >
      <template #default>
        <div class="match-list">
          <SkateCard v-for="match in skateMatches" :key="match.id" :match="match" />
        </div>
      </template>
    </el-tab-pane>

    <el-tab-pane
      label="Ginástica Artística"
      name="artistic-gymnastics"
    >
      <template #default>
        <div class="match-list">
          <ArtisticGymnasticsCard v-for="match in artisticGymnasticMatches" :key="match.id" :match="match" />
        </div>
      </template>
    </el-tab-pane>

  </el-tabs>
</template>

<script lang="ts">
  import FootballCard from './football/FootballCard.vue';
  import VolleyCard from './volleyball/VolleyCard.vue';
  import SkateCard from './skate/SkateCard.vue';
  import ArtisticGymnasticsCard from './artistic-gymnastics/ArtisticGymnasticsCard.vue';
  import { IFootball } from '../../interfaces/football';
  import { IVolley } from '../../interfaces/volley';
  import { ISkate } from '../../interfaces/skate';
  import { defineComponent, ref, onMounted } from 'vue';
  import { matchData } from '../../api/mocks/match-data';
  import { volleyData } from '../../api/mocks/volley-data';
  import { skateData } from '../../api/mocks/skate-data';
  import { artisticGymnasticsData } from '../../api/mocks/artistic-gymnastics-data';

  export default defineComponent({
    name: 'TabsWithMatchList',
    components: {
      FootballCard,
      VolleyCard,
      SkateCard,
      ArtisticGymnasticsCard,
    },
    setup() {
      const footballMatches = ref<IFootball[]>([]);
      const volleyballMatches = ref<IVolley[]>([]);
      const skateMatches = ref<ISkate[]>([]);
      const artisticGymnasticMatches = ref<IArtisticGymnastics[]>([]);
      const activeName = ref('football');

      onMounted(() => {
        setTimeout(() => {
          footballMatches.value = matchData.filter(match => match.sport === 'Futebol');
          volleyballMatches.value = volleyData.filter(match => match.sport === 'Vôlei');
          skateMatches.value = skateData.filter(match => match.sport === 'Skate');
          artisticGymnasticMatches.value = artisticGymnasticsData.filter(match => match.sport === 'Ginástica Artística');
        }, 1000);
      });

      const handleClick = (tab: any, event: Event) => {
        console.log(tab, event);
        activeName.value = tab.name;
      };

      return {
        activeName,
        footballMatches,
        volleyballMatches,
        skateMatches,
        artisticGymnasticMatches,
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
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    align-items: flex-start;
    padding: 20px;
    gap: 28px;
  }

  .match-list > * {
    width: 100%;
    max-width: 320px;
    margin-bottom: 20px;
  }
</style>
