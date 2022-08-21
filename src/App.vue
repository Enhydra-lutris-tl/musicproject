<template>
  <MusicList></MusicList>
  <MyTodo></MyTodo>
</template>

<script>
import MusicList from './components/MusicList.vue'
import MyTodo from "@/components/MyTodo";

export default {
  name: 'App',
  components: {
    MyTodo,
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
