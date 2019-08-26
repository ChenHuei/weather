<template>
  <div class="home">
    <div class="bgc"></div>
    <Topbar
      :city.sync="city"
      :citys="citys"
      @toggle="searchCapital"/>
    <List
      :list="list"
      @citySelect="selectCity"/>
    <City
      v-if="singleCity"
      :singleCity="singleCity"
      @close="closeCity"/>
  </div>
</template>

<script>
import Topbar from '@/components/Topbar'
import List from '@/components/List'
import City from '@/components/City'
import { API, SIX_CAPITAL } from '@/constants'
export default {
  name: 'Home',
  components: {
    Topbar,
    List,
    City
  },
  data () {
    return {
      isSixCapital: false,
      city: '',
      singleCity: '',
      oriList: [],
      API,
      SIX_CAPITAL
    }
  },
  watch: {
    city () {
      if (this.isSixCapital) this.isSixCapital = false
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
    }
  },
  methods: {
    searchCapital () {
      this.isSixCapital = true
    },
    selectCity (city) {
      this.singleCity = this.list.find(item => item.locationName === city)
    },
    closeCity () {
      this.singleCity = ''
    }
  },
  mounted () {
    fetch(this.API)
      .then(res => res.json())
      .then(res => {
        this.oriList = res.records.location
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
</style>
