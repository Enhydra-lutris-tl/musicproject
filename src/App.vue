<template>
  <MusicList></MusicList>
</template>

<script>
import MusicList from './components/MusicList.vue'

export default {
  name: 'App',
  components: {
    MusicList,
  }
}
</script>

<style>
*{
  padding: 0;
  margin: 0;
  text-align: center;
}
body{
  overflow: hidden;
}
</style>
