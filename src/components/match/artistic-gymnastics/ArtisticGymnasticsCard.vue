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
          <span class="match-date">
            {{ formattedDate }}
          </span>
        </div>
        <el-button
          class="mark-button"
          type="primary"
          round
          :disabled="isPastEvent"
          @click="redirectToGoogleCalendar"
        >
          Marcar no Calendário Google
        </el-button>
      </div>
    </el-card>
  </div>
</template>

<script lang="ts">
  import { IArtisticGymnastics } from '../../../interfaces/artistic-gymnastics' ;
  import { defineComponent, PropType } from 'vue';

  export default defineComponent({
    name: 'MatchCard',
    props: {
      match: {
        type: Object as PropType<IArtisticGymnastics>,
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
      isPastEvent(): boolean {
        const eventDateTime = new Date(`${this.match.date}T${this.match.time}`);
        const now = new Date();
        return eventDateTime < now;
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
          text: `[${this.match.sport} ${this.match.category}]`,
          dates: `${startDateTime}/${startDateTime}`,
          details: `[${this.match.category} - ${this.match.sport}]`,
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
      text-align: center;
      width: 100%;
    }

    .match-time {
      font-size: 18px;
      font-weight: bolder;
      margin-bottom: 16px;
    }
  }
  .mark-button {
    margin-top: 16px;
  }
</style>
