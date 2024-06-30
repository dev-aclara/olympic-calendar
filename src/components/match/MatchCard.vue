<template>
  <div>
    <el-card class="match-card">
      <div class="card-content">
        <div class="match-info">
          <span class="match-time">
            {{ match.time }}
          </span>
          <span class="match-date">
            {{ formattedDate }}
          </span>
          <span class="match-category">
            {{ match.category }}
          </span>
          <span class="match-stadium">
            {{ match.stadium }}
          </span>
        </div>
        <div class="teams">
          <div class="team">
            <span class="team-name">
              {{ match.homeTeam }}
            </span>
            <img
              :src="match.homeFlag"
              alt="home team flag"
              class="team-flag"
            />
          </div>
          <div class="versus">x</div>
          <div class="team">
            <img
              :src="match.awayFlag"
              alt="away team flag"
              class="team-flag"
            />
            <span class="team-name">
              {{ match.awayTeam }}
            </span>
          </div>
        </div>
        <el-button
          class="mark-button"
          type="primary"
          round
          @click="redirectToGoogleCalendar"
        >
          Marcar no Calendário Google
        </el-button>
      </div>
    </el-card>
  </div>
</template>

<script lang="ts">
import { Match } from '../../interfaces/match';
import { defineComponent, PropType } from 'vue';

export default defineComponent({
  name: 'MatchCard',
  props: {
    match: {
      type: Object as PropType<Match>,
      required: true,
    },
  },
  computed: {
    formattedDate(): string {
      const date = new Date(this.match.date);

      return date.toLocaleDateString('pt-BR');
    },
  },
  methods: {
    redirectToGoogleCalendar() {
      const baseUrl = 'https://calendar.google.com/calendar/r/eventedit';
      const startDateTime = new Date(`${this.match.date}T${this.match.time}`).toISOString().replace(/[-:.]/g, '');

      const params = new URLSearchParams({
        text: `[${this.match.sport} ${this.match.category}] ${this.match.homeTeam} x ${this.match.awayTeam}`,
        dates: `${startDateTime}/${startDateTime}`,
        details: `[${this.match.category} - ${this.match.sport}] - ${this.match.homeTeam} x ${this.match.awayTeam} (${this.match.group})`,
        location: this.match.stadium,
      });

      const url = `${baseUrl}?${params.toString()}`;
      window.open(url, '_blank');
    },
  },
});
</script>

<style scoped lang="scss">
  .match-card {
    max-width: 320px;
    margin: 0 auto;
    padding: 10px;
    display: flex;
    justify-content: center;
    border-radius: 20px;

    span {
      font-size: 16px;
    }
  }

  .card-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
  }

  .match-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 8px;

    .match-date,
    .match-category,
    .match-stadium,
    .match-time {
      margin: 2px 0;
    }

    .match-time {
      font-size: 18px;
      font-weight: bolder;
      margin-bottom: 16px;
    }
  }

  .teams {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
  }

  .team {
    display: flex;
    align-items: center;
    margin: 0 10px;

    .team-flag {
      width: 38px;
      height: 26px;
      margin: 0 10px;
    }
  }

  .versus {
    font-size: 20px;
    font-weight: bolder;
  }

  .mark-button {
    margin-top: 16px;
  }
</style>
