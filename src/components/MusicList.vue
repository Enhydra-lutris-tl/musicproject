<template>
<div>
  <ul class="listBox">
    <li class="list1" @click="musicPlay()">
      <audio :src="music[0]" ref="list1"></audio></li>
    <li class="list2" @click="musicPlay()">
      <audio :src="music[1]"></audio></li>
    <li class="list3" @click="musicPlay()">
      <audio :src="music[2]"></audio></li>
    <li class="list4" @click="musicPlay()">
      <audio :src="music[3]"></audio></li>
  </ul>
</div>
</template>

<script>
export default {
  name: "MusicList",
  data(){
    return{
      music:[
          require('@/assets/music/1.mp3'),
          require('@/assets/music/2.mp3'),
          require('@/assets/music/3.mp3'),
          require('@/assets/music/4.mp3'),
          require('@/assets/music/5.mp3'),
          require('@/assets/music/6.mp3'),
          require('@/assets/music/7.mp3'),
          require('@/assets/music/8.mp3'),
      ]
    }
  },
  methods:{
    musicPlay(){
        this.$refs.list1.play()
        setTimeout(()=>{
          var a = Math.floor(Math.random()*8)
          this.$refs.list1.src = this.music[a]
        },1000)
    }
  }
}
</script>

<style scoped>
.listBox{
  position: absolute;
  height: 400px;
  width: 400px;
  top: calc(50% - 200px);
  left: calc(50% - 200px) ;
}
.listBox li{
  position: absolute;
  height: 40px;
  width: 40px;
  background: aquamarine;
  list-style: none;
  transform: rotate(45deg);
}
.list1{
  left: calc(50% - 20px);
}
.list2{
  top: calc(50% - 20px);
  right: 0;
}
.list3{
  left: calc(50% - 20px);
  bottom: 0;
}
.list4{
  top: calc(50% - 20px);
}
</style>