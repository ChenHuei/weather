<template>
  <div id="app">
    <router-view :key="$route.fullPath"/>
  </div>
</template>

<style lang="scss">
@import './styles/import';
#app {
  @include size(calc(100vw -15px), 100vh);
  font-family: $font-family;
  color: color(black);
}

</style>
