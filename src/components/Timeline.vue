<template>
  <div class="timeline-container" :style="`--numEvents:${numEvents}`">
    <div class="days" :style="{width: getWidth()}" v-if="days.length">
      <div
          class="day"
          v-for="day in days"
          :key="day.abstract.ts"
          :ref="day.isToday ? 'today' : 'day'"
          :class="[{today: day.isToday}]"
      >
        <div class="label">
          {{ day.label }}
        </div>

        <div class="events" v-if="day.eventStart.length">
          <div
              class="event"
              v-for="event in day.eventStart"
              :key="event.id"
              :data-slot="event.slot"
              :data-duration="event.duration"
              :style="`--duration:${event.duration}; --slot:${event.slot};`"
          >
            {{ event.label }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import events from '../events';
  import Calendar from 'buttercal-core';
  const calendar = new Calendar();
  calendar.addCalEvents(events);

  export default {
    name: "Timeline",

    data() {
      return {
        days: [],
        width: '0',
        numEvents: events.length
      }
    },
    
    created() {
      const mql = window.matchMedia('(min-width: 800px)');
      this.width = mql.matches ? `${(window.innerWidth/3)*this.days}px` : `${(window.innerWidth/7)*this.days}px`;

      mql.addListener(({ matches }) => {
        this.width = matches ? `${(window.innerWidth/3)*this.days}px` : `${(window.innerWidth/7)*this.days}px`;
      });

      this.days = [
        ...calendar.renderedMonths[0].days.filter(day => day.inMonth),
        ...calendar.renderedMonths[1].days.filter(day => day.inMonth),
        ...calendar.renderedMonths[2].days.filter(day => day.inMonth),
      ];
    },

    mounted() {
      this.$refs.today[0].scrollIntoView({ block: "center" });
    },

    methods: {
      getWidth() {
        return `${(window.innerWidth/7)*this.days}px`;
      }
    }
  }
</script>

<style scoped lang="scss">
  @import '../scss/imports';

  .timeline-container {
    position: relative;
    overflow-y: scroll;
    -webkit-overflow-scrolling: touch;

    width: 100%;
    height: calc(100vh - 100px);

    border-top: 1px solid $border-dark;
    border-bottom: 1px solid $border-dark;
    box-shadow: 0 1px 0 rgba(245, 245, 245, 0.1), 0 -1px 0 rgba(245, 245, 245, 0.1);
  }

  .days {
    display: flex;
    flex-flow: row nowrap;
  }

  .day {
    position: relative;
    height: calc(100vh - 100px);
    border-right: 2px solid $dark;
    flex-grow: 1;
    flex-shrink: 0;
    background: $slategrey;
    width: 33vw;
    min-width: 33vw;
    flex-basis: 33vw;

    @media (min-width: 800px) {
      width: 14vw;
      min-width: 14vw;
      flex-basis: 14vw;
    }

    &.today {
      background: lighten($slategrey, 10%);
    }

    .label {
      font-size: 18px;
    }
  }

  .event {
    cursor: pointer;
    text-align: left;
    z-index: 1;
    color: #fff;
    padding: 7px;
    border-radius: 6px;
    position: absolute;
    top: calc(var(--slot) * (60vh / 6));
    left: 50%;
    transform: translateX(-50%);
    background: $dark;
    font-size: 14px;
    line-height: 1.2;
    width: calc((14vw * var(--duration) - 15px));
    min-width: calc(100% - 15px);

    @media (min-width: 800px) {
      font-size: 16px;
      padding: 10px;
    }
  }
</style>