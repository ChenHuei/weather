<template>
  <div :class="cityClassHandler">
    <div
      class="bgc"
      @click="close">
    </div>
    <div class="container">
      <div class="title">
        {{titleHandler}}
      </div>
      <div
        class="cancel"
        @click="close"
      >
        X
      </div>
      <div class="boxs">
        <div
          class="box"
          v-for="time in times"
          :key="time"
        >
          <div class="header">
            <span class="date">{{recentStart(time)}} ~ {{recentEnd(time)}}</span>
          </div>
          <div class="content">
            <div class="description">{{descriptionHandler(time)}}</div>
            <div class="weather">
              <div class="temperature">{{temperatureHandler(time)}}</div>
              <div class="rain">{{rainHandler(time)}}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'City',
  data () {
    return {
      isLoading: true
    }
  },
  props: {
    singleCity: {
      type: [String, Object],
      required: true
    }
  },
  computed: {
    cityClassHandler () {
      return {
        city: true,
        loading: this.isLoading
      }
    },
    titleHandler () {
      return this.singleCity.locationName
    },
    times () {
      return this.singleCity.weatherElement[0].time.length
    },
    today () {
      return new Date().getDate()
    }
  },
  methods: {
    close () {
      this.$emit('close')
    },
    recentStart (time) {
      const start = new Date(this.singleCity.weatherElement[0].time[time - 1].startTime)
      const date = start.getDate() === this.today ? '今日' : '明日'
      return `${date} ${start.getHours()}:00`
    },
    recentEnd (time) {
      const end = new Date(this.singleCity.weatherElement[0].time[time - 1].endTime)
      const date = end.getDate() === this.today ? '今日' : '明日'
      return `${date} ${end.getHours()}:00`
    },
    descriptionHandler (time) {
      return this.singleCity.weatherElement[0].time[time - 1].parameter.parameterName
    },
    temperatureHandler (time) {
      const min = this.singleCity.weatherElement[2].time[time - 1].parameter.parameterName
      const max = this.singleCity.weatherElement[4].time[time - 1].parameter.parameterName
      return `${min}℃ ~ ${max}℃`
    },
    rainHandler (time) {
      const percent = this.singleCity.weatherElement[1].time[time - 1].parameter.parameterName
      return `降雨機率：${percent}%`
    }
  },
  mounted () {
    setTimeout(() => {
      this.isLoading = false
    }, 100)
  }
}
</script>

<style lang="scss" scoped>
@import "../styles/import";

.city {
  @include size(100%);
  @include flexCenter;
  position: fixed;
  left: 0;
  top: 0;
  transition: .4s cubic-bezier(.17,.67,.22,.93);
  &.loading {
    top: -50%;
  }
  > .bgc {
    @include size(100%);
    position: absolute;
    left: 0;
    top: 0;
    background-color: color(black);
    opacity: 0.6;
  }
}

.container {
  @include size(80%, auto);
  position: relative;
  padding: 5%;
  background-color: color(white);
  border-radius: 8px;
  overflow: hidden;
  z-index: 10;
  > .cancel {
    @include size(36px);
    @include flexCenter;
    position: absolute;
    right: 12px;
    top: 12px;
    font-size: 20px;
    border: 1px solid color(grey);
    border-radius: 50%;
    transition: .5s;
    opacity: 0.6;
    cursor: pointer;
    &:hover {
      color: color(white);
      background-color: color(red);
      border: 1px solid color(red);
      opacity: 1;
    }
  }
  > .title {
    @include size(100%, auto);
    @include flexCenter;
    position: relative;
    font-size: 32px;
    font-weight: 500;
  }
}
.boxs {
  @include size(100%, auto);
  @include flexCenter;
  flex-wrap: wrap;
  justify-content: flex-start;
  margin-top: 40px;
  > .box {
    @include size(calc(90% / 3), auto);
    margin: 0 5% 5% 0;
    border: 1px solid color(grey);
    border-radius: 8px;
    overflow: hidden;
    &:last-child {
      margin-right: 0;
    }
  }
}
.header {
  @include size(100%, 48px);
  @include flexCenter;
  padding: 12px 16px;
  background-color: color(grey_light);
  > .date {
    font-size: 16px;
  }
}
.content {
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
  .boxs {
    > .box {
      @include size(calc(90% / 2), auto);
    }
  }
}

@media screen and (max-width: 767px) {
  .boxs {
    > .box {
      @include size(100%, auto);
      margin: 0 0 5% 0;
    }
  }
}
</style>
