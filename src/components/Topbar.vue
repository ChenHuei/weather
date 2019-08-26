<template>
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
        v-model="cityHandler">
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
</template>

<script>
export default {
  name: 'Topbar',
  props: {
    city: {
      type: String,
      required: true
    },
    citys: {
      type: Array,
      required: true
    }
  },
  computed: {
    cityHandler: {
      get () {
        return this.city
      },
      set (val) {
        this.$emit('update:city', val)
      }
    }
  },
  methods: {
    searchCapital () {
      this.$emit('toggle')
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../styles/import";

.topbar {
  @include size(100%, 20%);
  @include flexCenter;
  flex-direction: column;
  > .title {
    font-size: 32px;
    margin: 0 0 28px 0;
  }
  > .input {
    @include size(100%, auto);
    @include flexCenter;
  }
}
.search {
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
.select {
  @include size(40%, 40px);
  color: color(grey);
  background: color(white);
  outline: none;
}
</style>
