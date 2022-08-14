<template>
<div class="back">

  <canvas
      id="canvasBack"
  ></canvas>
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
        require('@/assets/music/a-.mp3'),
        require('@/assets/music/b-.mp3'),
        require('@/assets/music/c-.mp3'),
        require('@/assets/music/d-.mp3'),
        require('@/assets/music/e-.mp3'),
        require('@/assets/music/f-.mp3'),
        require('@/assets/music/g-.mp3'),
        require('@/assets/music/a-1.mp3'),
        require('@/assets/music/b-1.mp3'),
        require('@/assets/music/c-1.mp3'),
        require('@/assets/music/d-1.mp3'),
        require('@/assets/music/e-1.mp3'),
        require('@/assets/music/f-1.mp3'),
        require('@/assets/music/g-1.mp3'),
        require('@/assets/music/a-2.mp3'),
        require('@/assets/music/b-2.mp3'),
        require('@/assets/music/c-2.mp3'),
        require('@/assets/music/d-2.mp3'),
        require('@/assets/music/e-2.mp3'),
        require('@/assets/music/f-2.mp3'),
        require('@/assets/music/g-2.mp3'),
        require('@/assets/music/a3.mp3'),
        require('@/assets/music/b3.mp3'),
        require('@/assets/music/c3.mp3'),
        require('@/assets/music/d3.mp3'),
        require('@/assets/music/e3.mp3'),
        require('@/assets/music/f3.mp3'),
        require('@/assets/music/g3.mp3'),
          // require('@/assets/music/e3.mp3'),
          // require('@/assets/music/f3.mp3'),
          // require('@/assets/music/g3.mp3'),
      ],
      iconSize:80,
      canvasSize:{width:'',height:''},
      canvasColor:'rgba(108,234,232)',
    }
  },
  methods:{
    // 调整画布尺寸
    drawBack(cx,cy){
      var canvas = document.getElementById('canvasBack')
      canvas.height = document.documentElement.clientHeight
      canvas.width = document.documentElement.clientWidth
      var ctx = canvas.getContext('2d')
      // 清除画布内容，防止拉伸
      ctx.clearRect(0,0,canvas.width,canvas.height)
      var grd = ctx.createLinearGradient(0,0,canvas.width,canvas.height,)
      grd.addColorStop(0,'#5FFBF1')
      grd.addColorStop(1,'#D16BA5')
      ctx.fillStyle=grd;
      ctx.fillRect(0,0,canvas.width,canvas.height)
      var a = ctx.getImageData(cx,cy,1,1).data
      this.canvasColor = `rgba(${a[0] - 30},${a[1] - 20},${a[2] - 20},${a[3]})`
      console.log(this.canvasColor)
    },
    // 获取窗口高宽
    boxCoord(index){
      const bodyHeight = document.documentElement.clientHeight
      const bodyWidth = document.documentElement.clientWidth
      if (bodyWidth <= 428){
        this.iconSize = 40
      }else {
        this.iconSize = 80
      }
      //音符大小修改
      for (let list = 0; list < this.$refs.list.length; list++){
        this.$refs.list[list].style.width = this.iconSize + 'px'
        this.$refs.list[list].style.height = this.iconSize + 'px'
        this.$refs.list[list].style.lineHeight = this.iconSize + 'px'
      }
      // 随机音符元素位置
      var boxTop = Math.floor(Math.random()*bodyHeight)
      var boxLeft = Math.floor(Math.random()*bodyWidth)
      if (boxTop <= 80){
        boxTop = 80
      }else if(bodyHeight - boxTop <= 80 ){
        boxTop = bodyHeight - 80
      }
      if (boxLeft <= 80){
        boxLeft = 80
      }else if(bodyWidth - boxLeft <= 80 ){
        boxLeft = bodyWidth - 80
      }
      this.drawBack(boxLeft,boxTop)
      this.$refs.list[index].style.top = boxTop + 'px'
      this.$refs.list[index].style.left = boxLeft + 'px'
    },

    iconSpin(index){
      //选中后的样式改变
      this.$refs.list[index].style.background = this.canvasColor
      this.$refs.list[index].style.boxShadow = `0 0 0 0 ${this.canvasColor}`
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

    //随机读取一个音效
    musicPlay(index){
      var musicSrc = Math.floor(Math.random()* 28)
      new Audio(this.music[musicSrc]).play()
      this.boxCoord(index)
      this.iconSpin(index)
    }
  },
  mounted() {
    this.drawBack(this.canvasSize["width"],this.canvasSize["height"])
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
    window.onresize = () =>{
      this.drawBack(this.canvasSize["width"],this.canvasSize["height"])
    }
  }
}
</script>

<style scoped>
.back{
  height: 100vh;
  width: 100%;
  overflow: hidden;
  /*background-image: linear-gradient(to left top, #d16ba5, #c777b9, #ba83ca, #aa8fd8, #9a9ae1, #8aa7ec, #79b3f4, #69bff8, #52cffe, #41dfff, #46eefa, #5ffbf1);*/
}
.list{
  position: absolute;
  height: 80px;
  width: 80px;
  top: calc(50% - 40px);
  left: calc(50% - 40px);
  line-height: 80px;
  transition: 0.5s;
}
.iconfont{
  display: block;
  position: absolute;
  height: 100%;
  width: 100%;
  font-size: 50px;
  transition: 0.5s;
}

.list,
.list:before,
.list:after{
  border-radius: 50%;
  background: transparent;
  animation: 2.5s shadowShow infinite var(--s,0s);
  box-shadow: 0 0 0 0 rgba(147, 147, 147, 0.5);
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