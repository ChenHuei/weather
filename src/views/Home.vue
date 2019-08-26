<template>
  <div class="home">
    <div class="bgc"></div>
    <div class="topbar">
      <h2 class="title">天氣預報</h2>
      <div class="input">
        <button
          class="search"
          @click="searchCapital"
        >
          查詢六都
        </button>
        <select
          class="select"
          :value="city"
          @input="optionHandler">
          <option value="">全部縣市</option>
          <option
            class="option"
            v-for="item in citys"
            :key="item"
            :value="item"
          >
            {{item}}
          </option>
        </select>
      </div>
    </div>
    <div class="list">
      <div
        class="item"
        v-for="item in list"
        :key="item.locationName"
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
  </div>
</template>

<script>
import { API, SIX_CAPITAL } from '../constants'
export default {
  name: 'Home',
  data () {
    return {
      isSixCapital: false,
      city: '',
      oriList: [],
      API,
      SIX_CAPITAL
    }
  },
  computed: {
    list () {
      if (this.isSixCapital) {
        return this.oriList.filter(item => this.SIX_CAPITAL.includes(item.locationName))
      }
      return this.city === '' ? this.oriList : this.oriList.filter(item => this.city === item.locationName)
    },
    citys () {
      return this.oriList.map(item => item.locationName)
    },
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
    searchCapital () {
      this.isSixCapital = true
    },
    optionHandler (e) {
      this.isSixCapital = false
      this.city = e.target.value
    },
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
    }
  },
  mounted () {
    fetch(this.API)
      .then(res => res.json())
      .then(res => {
        this.oriList = res.records.location
        console.log(this.oriList)
      })
  }
}
</script>

<style lang="scss" scoped>
@import "../styles/import";

.home {
  @include size(100%);
  position: relative;
}

.bgc {
  @include size(100%);
  @include backgroundImage('https://images.unsplash.com/photo-1556515268-97d056bdb5a7?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1348&q=80');
  position: fixed;
  left: 0;
  top: 0;
  opacity: 0.8;
  z-index: -1;
}

.topbar {
  @include size(100%, 15%);
  @include flexCenter;
  flex-direction: column;
  > .title {
    font-size: 32px;
    margin: 0 0 20px 0;
  }
  > .input {
    @include size(100%, auto);
    @include flexCenter;
    > .search {
      @include size(100px, 40px);
      margin-right: 32px;
      font-weight: 700;
      color: color(grey);
      background-color: color(white);
      border: 1px solid color(grey_light);
      border-radius: 8px;
      transition: .5s;
      outline: none;
      cursor: pointer;
      &:hover {
        color: color(white);
        background-color: color(grey_light);
      }
    }
    > .select {
      @include size(40%, 40px);
      color: color(grey);
      background: color(white);
      outline: none;
    }
  }
}

.list {
  @include size(100%, auto);
  @include flexCenter;
  justify-content: flex-start;
  flex-wrap: wrap;
  padding: 40px 15%;
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
    > .header {
      @include size(100%, 48px);
      @include flexCenter;
      padding: 12px 16px;
      justify-content: space-between;
      background-color: color(grey_light);
      > .city {
        font-size: 20px;
        font-weight: 700;
      }
    }
    > .container {
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
  }
}
</style>
