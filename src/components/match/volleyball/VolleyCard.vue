<template>
  <div>
    <el-card class="match-card">
      <div class="card-content">
        <div class="match-info">
          <span class="match-time">
            {{ match.time }}
          </span>
          <span class="match-category">
            {{ match.category }}
          </span>
          <span class="match-group">
            {{ match.group }}
          </span>
          <span class="match-date">
            {{ formattedDate }}
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
  import { IVolley } from '../../../interfaces/volley';
  import { defineComponent, PropType } from 'vue';

  export default defineComponent({
    name: 'MatchCard',
    props: {
      match: {
        type: Object as PropType<IVolley>,
        required: true,
      },
    },
    computed: {
      formattedDate(): string {
        const dateParts = this.match.date.split('-');
        const day = dateParts[2];
        const month = dateParts[1];
        const year = dateParts[0];

        return `${day}/${month}/${year}`;
      },
    },
    methods: {
      redirectToGoogleCalendar() {
        const baseUrl = 'https://calendar.google.com/calendar/r/eventedit';

        const startDate = new Date(`${this.match.date}T${this.match.time}`);
        const startISODateTime = startDate.toISOString();

        const startDateTime = startISODateTime.substring(0, 4) +
          startISODateTime.substring(5, 7) +
          startISODateTime.substring(8, 10) +
          'T' +
          startISODateTime.substring(11, 13) +
          startISODateTime.substring(14, 16) +
          startISODateTime.substring(17, 19) +
          'Z';

        const params = new URLSearchParams({
          text: `[${this.match.sport} ${this.match.category}] ${this.match.homeTeam} x ${this.match.awayTeam}`,
          dates: `${startDateTime}/${startDateTime}`,
          details: `[${this.match.category} - ${this.match.sport}] - ${this.match.homeTeam} x ${this.match.awayTeam} (${this.match.group})`,
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
