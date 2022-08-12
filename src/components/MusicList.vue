<template>
<div class="back">
  <div
      v-for="(a,index) in 5"
      :key="index"
      class="list"
      @click="musicPlay(index)"
      ref="list"
  >
    <i class="iconfont icon-icon-test"></i>
  </div>
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
          require('@/assets/music/502339.mp3'),
      ]
    }
  },
  methods:{
    boxCoord(index){
      // 获取窗口高宽
      const bodyHeight = document.documentElement.clientHeight
      const bodyWidth = document.documentElement.clientWidth
      // 随机元素位置
      var boxTop = Math.floor(Math.random()*bodyHeight)
      var boxLeft = Math.floor(Math.random()*bodyWidth)
      this.$refs.list[index].style.top = boxTop + 'px'
      this.$refs.list[index].style.left = boxLeft + 'px'
    },
    iconSpin(index){
      //选中后的样式改变
      this.$refs.list[index].style.background = '#5FFBF1'
      this.$refs.list[index].style.boxShadow = '0 0 0 0 rgba(95,251,241,0.5)'
     setTimeout(()=>{
       this.$refs.list[index].style.background = 'transparent'
      },500)
      // 音符图标转圈逻辑
      this.$refs.list[index].firstChild.style.transition = '0.5s'
      this.$refs.list[index].firstChild.classList.add('iconSpin')
      setTimeout(()=>{
        this.$refs.list[index].firstChild.style.transition = '0s'

        this.$refs.list[index].firstChild.classList.remove('iconSpin')
      },500)
    },
    musicPlay(index){
      //随机读取一个音效
      var musicSrc = Math.floor(Math.random()*9)
      new Audio(this.music[musicSrc]).play()
      this.boxCoord(index)
      this.iconSpin(index)
    }
  },
  mounted() {
    setTimeout(()=>{
      // 随机元素位置
      for (let e = 0; e < this.$refs.list.length; e++) {
        this.boxCoord(e)
      }
    },300)
    document.onkeydown = ()=>{
      if (window.event.keyCode === 32){
        let domIndex = Math.floor(Math.random()*5)
        this.musicPlay(domIndex)
      }
    }
    document.onclick = ()=>{
        let domIndex = Math.floor(Math.random()*5)
        this.musicPlay(domIndex)
    }
  }
}
</script>

<style scoped>
.back{
  height: 100vh;
  width: 100%;
  overflow: hidden;
  background-image: linear-gradient(to left top, #d16ba5, #c777b9, #ba83ca, #aa8fd8, #9a9ae1, #8aa7ec, #79b3f4, #69bff8, #52cffe, #41dfff, #46eefa, #5ffbf1);
}
.list{
  position: absolute;
  height: 80px;
  width: 80px;
  top: calc(50% - 40px);
  left: calc(50% - 40px);
  transition: 0.5s;
}
.iconfont{
  display: block;
  position: absolute;
  height: 100%;
  width: 100%;
  font-size: 50px;
  line-height: 80px;
  transition: 0.5s;
}

.list,
.list:before,
.list:after{
  border-radius: 50%;
  background: transparent;
  animation: 3s shadowShow infinite var(--s,0s);
  box-shadow: 0 0 0 0 rgba(108, 108, 108, 0.5);
}
.list:before,
.list:after{
  content: '';
  position: absolute;
  left: 0;
  height: 100%;
  width: 100%;

}
.list:before{
  --s:.5s
}
.list:after{
  --s:1s
}
@keyframes shadowShow{
  to {
    box-shadow: 0 0 0 60px transparent;
  }
}
.iconSpin {
    transform: rotate(360deg);
}
</style>