<template>
  <div class="list">
    <div
      class="item"
      v-for="item in list"
      :key="item.locationName"
      @click="clickHandler(item)"
    >
      <div class="header">
        <span class="city">{{item.locationName}}</span>
        <span class="date">{{recentStart}} ~ {{recentEnd}}</span>
      </div>
      <div class="container">
        <div class="description">{{descriptionHandler(item)}}</div>
        <div class="weather">
          <div class="temperature">{{temperatureHandler(item)}}</div>
          <div class="rain">{{rainHandler(item)}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'List',
  props: {
    list: {
      type: Array,
      required: true
    }
  },
  computed: {
    today () {
      return new Date().getDate()
    },
    recentStart () {
      const start = new Date(this.list[0].weatherElement[0].time[0].startTime)
      const date = start.getDate() === this.today ? '今日' : '明日'
      return `${date} ${start.getHours()}:00`
    },
    recentEnd () {
      const end = new Date(this.list[0].weatherElement[0].time[0].endTime)
      const date = end.getDate() === this.today ? '今日' : '明日'
      return `${date} ${end.getHours()}:00`
    }
  },
  methods: {
    descriptionHandler (item) {
      return item.weatherElement[0].time[0].parameter.parameterName
    },
    temperatureHandler (item) {
      const min = item.weatherElement[2].time[0].parameter.parameterName
      const max = item.weatherElement[4].time[0].parameter.parameterName
      return `${min}℃ ~ ${max}℃`
    },
    rainHandler (item) {
      const percent = item.weatherElement[1].time[0].parameter.parameterName
      return `降雨機率：${percent}%`
    },
    clickHandler (item) {
      this.$emit('citySelect', item.locationName)
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../styles/import";

.list {
  @include size(100%, auto);
  @include flexCenter;
  justify-content: flex-start;
  flex-wrap: wrap;
  padding: 20px 10% 40px;
  > .item {
    @include size(calc(90% / 3), 148px);
    margin: 0 5% 5% 0;
    background-color: color(white);
    border-radius: 8px;
    transition: .5s;
    overflow: hidden;
    cursor: pointer;
    &:nth-child(3n + 3) {
      margin: 0 0 5% 0;
    }
    &:hover {
      transform: translateY(-8px);
    }
  }
}
.header {
  @include size(100%, 48px);
  @include flexCenter;
  padding: 12px 16px;
  justify-content: space-between;
  background-color: color(grey_light);
  > .city {
    font-size: 20px;
    font-weight: 700;
  }
  > .date {
    font-size: 16px;
  }
}
.container {
  @include size(100%, 100px);
  @include flexCenter;
  align-items: flex-start;
  flex-direction: column;
  padding: 12px 16px;
  > .description {
    @include size(100%, auto);
  }
  > .weather {
    @include size(100%, auto);
    @include flexCenter;
    justify-content: flex-start;
    margin-top: 12px;
    > .temperature {
      margin-right: 20px;
    }
  }
}

@media screen and (min-width: 768px) and (max-width: 1024px) {
  .list {
    > .item {
      @include size(calc(90% / 2), 148px);
      &:nth-child(3n + 3) {
        margin: 0 5% 5% 0;
      }
      &:nth-child(2n + 2) {
        margin: 0 0 5% 0;
      }
    }
  }
}

@media screen and (max-width: 767px) {
  .list {
    > .item {
      @include size(100%, 148px);
      margin: 0 0 5% 0;
      &:nth-child(3n + 3) {
        margin: 0 0 5% 0;
      }
      &:nth-child(2n + 2) {
        margin: 0 0 5% 0;
      }
    }
  }
  .header {
    > .city {
      font-size: 16px;
    }
    > .date {
      font-size: 12px;
    }
  }
}
</style>
