<template>
<div class="back">

  <canvas
      id="canvasBack"
      @click="musicClicked"
  ></canvas>
  <div class="tipsTitleBox" v-show="!mobileShow">
    <span>键盘A,高音</span>
    <span>键盘W,中音</span>
    <span>键盘D,低音</span>
    <span>键盘空格,变音</span>
  </div>
  <div
      v-for="(a,index) in 5"
      :key="index"
      class="list"
      @click="musicPlay(index)"
      ref="list"
  >
    <i class="iconfont icon-icon-test" ref="iconfont"></i>
  </div>
  <div class="musicClickLow" v-show="mobileShow" @click="musicClicked(1)">低</div>
  <div class="musicClickMiddle" v-show="mobileShow" @click="musicClicked(2)">中</div>
  <div class="musicClickHeight" v-show="mobileShow" @click="musicClicked(3)">高</div>
  <div class="musicClickChange" v-show="mobileShow" @click="musicClicked(4)">变</div>
</div>
</template>

<script>
export default {
  name: "MusicList",
  data(){
    return{
      mobileShow:false,
      highPitch:[
        'bigCharacterTwo',
        'bigCharacterOne',
        'bigCharacter',
      ],
      altoVoice:[
        'smallCharacter',
        'smallCharacterOne',
        'smallCharacterTwo'
      ],
      bass:[
        'smallCharacterThree',
        'smallCharacterFour',
        'smallCharacterFive'
      ],
      music:{
        natureMusic:{
          bigCharacterTwo:[
            require('@/assets/music/A2.mp3'),
            require('@/assets/music/B2.mp3'),
          ],
          bigCharacterOne:[
              require('@/assets/music/C1.mp3'),
              require('@/assets/music/D1.mp3'),
              require('@/assets/music/E1.mp3'),
              require('@/assets/music/F1.mp3'),
              require('@/assets/music/G1.mp3'),
              require('@/assets/music/A1.mp3'),
              require('@/assets/music/B1.mp3'),
          ],
          bigCharacter:[
              require('@/assets/music/C.mp3'),
              require('@/assets/music/D.mp3'),
              require('@/assets/music/E.mp3'),
              require('@/assets/music/F.mp3'),
              require('@/assets/music/G.mp3'),
              require('@/assets/music/A.mp3'),
              require('@/assets/music/B.mp3'),
          ],
          smallCharacter:[
              require('@/assets/music/c-.mp3'),
              require('@/assets/music/d-.mp3'),
              require('@/assets/music/e-.mp3'),
              require('@/assets/music/f-.mp3'),
              require('@/assets/music/g-.mp3'),
              require('@/assets/music/a-.mp3'),
              require('@/assets/music/b-.mp3'),
          ],
          smallCharacterOne:[
              require('@/assets/music/c-1.mp3'),
              require('@/assets/music/d-1.mp3'),
              require('@/assets/music/e-1.mp3'),
              require('@/assets/music/f-1.mp3'),
              require('@/assets/music/g-1.mp3'),
              require('@/assets/music/a-1.mp3'),
              require('@/assets/music/b-1.mp3'),
          ],
          smallCharacterTwo:[
              require('@/assets/music/c-2.mp3'),
              require('@/assets/music/d-2.mp3'),
              require('@/assets/music/e-2.mp3'),
              require('@/assets/music/f-2.mp3'),
              require('@/assets/music/g-2.mp3'),
              require('@/assets/music/a-2.mp3'),
              require('@/assets/music/b-2.mp3'),
          ],
          smallCharacterThree:[
              require('@/assets/music/c3.mp3'),
              require('@/assets/music/d3.mp3'),
              require('@/assets/music/e3.mp3'),
              require('@/assets/music/f3.mp3'),
              require('@/assets/music/g3.mp3'),
              require('@/assets/music/a3.mp3'),
              require('@/assets/music/b3.mp3'),
          ],
          smallCharacterFour:[
              require('@/assets/music/c4.mp3'),
              require('@/assets/music/d4.mp3'),
              require('@/assets/music/e4.mp3'),
              require('@/assets/music/f4.mp3'),
              require('@/assets/music/g4.mp3'),
              require('@/assets/music/a4.mp3'),
              require('@/assets/music/b4.mp3'),
          ],
          smallCharacterFive:[
              require('@/assets/music/c5.mp3'),
          ],
        },
        changeMusic:[
            require('@/assets/music/A+.mp3'),
            require('@/assets/music/A+1.mp3'),
            require('@/assets/music/A+2.mp3'),
            require('@/assets/music/C+.mp3'),
            require('@/assets/music/C+1.mp3'),
            require('@/assets/music/D+.mp3'),
            require('@/assets/music/D+1.mp3'),
            require('@/assets/music/F+.mp3'),
            require('@/assets/music/F+1.mp3'),
            require('@/assets/music/G+.mp3'),
            require('@/assets/music/G+1.mp3'),
            require('@/assets/music/a-+.mp3'),
            require('@/assets/music/a-+1.mp3'),
            require('@/assets/music/a-+2.mp3'),
        ]

      },
      iconSize:{icon:80,music:50},
      canvasSize:{width:'',height:''},
      canvasColor:'rgba(108,234,232)',
    }
  },
  methods:{
    // 随机选择一个音符
    musicRange(){
      return Math.floor(Math.random()*5)
    },
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
    },

    // 获取窗口高宽
    musicSize(){
      // const bodyHeight = document.documentElement.clientHeight
      const bodyWidth = document.documentElement.clientWidth
      if (bodyWidth <= 428){
        this.iconSize["icon"] = 40
        this.iconSize["music"] = 30
        this.mobileShow = true
      }else {
        this.iconSize["icon"] = 80
        this.iconSize["music"] = 50
        this.mobileShow = false
      }
      //音符圆环大小修改
      for (let list = 0; list < this.$refs.list.length; list++){
        this.$refs.list[list].style.width = this.iconSize["icon"] + 'px'
        this.$refs.list[list].style.height = this.iconSize["icon"] + 'px'
        this.$refs.list[list].style.lineHeight = this.iconSize["icon"] + 'px'
      }
      // 音符大小修改
      for (let iconfont = 0; iconfont < this.$refs.iconfont.length; iconfont++){
        this.$refs.iconfont[iconfont].style.fontSize = this.iconSize["music"] + 'px'
      }
    },
    boxCoord(index){
      const bodyHeight = document.documentElement.clientHeight
      const bodyWidth = document.documentElement.clientWidth
      // 随机音符元素位置
      var boxTop = Math.floor(Math.random()*bodyHeight)
      var boxLeft = Math.floor(Math.random()*bodyWidth)
      //判断页面宽度
      if (bodyWidth <= 428){
        if(bodyHeight - boxTop <= 210 ){
          boxTop = bodyHeight - 210
        }
      }else {
        if(bodyHeight - boxTop <= 80 ){
          boxTop = bodyHeight - 80
        }
      }
      if (boxTop <= 80){
          boxTop = 80
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
    musicPlay(index,musicKindred){
      // 自然音和变音区分
      var musicSrc
      if (musicKindred === 1){
        const musicKind = this.highPitch[Math.floor(Math.random()* 2)]
        const musicJust = this.music["natureMusic"][musicKind]
        musicSrc = musicJust[Math.floor(Math.random()* (musicJust.length - 1))]
      }else if(musicKindred === 2){
        const musicKind = this.altoVoice[Math.floor(Math.random()* 2)]
        const musicJust = this.music["natureMusic"][musicKind]
        musicSrc = musicJust[Math.floor(Math.random()* (musicJust.length - 1))]
      }else if(musicKindred === 3){
        const musicKind = this.altoVoice[Math.floor(Math.random()* 2)]
        const musicJust = this.music["natureMusic"][musicKind]
        musicSrc = musicJust[Math.floor(Math.random()* (musicJust.length - 1))]
      }else {
        musicSrc = this.music["changeMusic"][Math.floor(Math.random()* 13)]
      }
      new Audio(musicSrc).play()
      this.boxCoord(index)
      this.iconSpin(index)
    },
    //点击触发
    musicClicked(musicKindred){
      this.musicPlay(this.musicRange(),musicKindred)
    }
  },
  mounted() {
    this.musicSize()
    this.drawBack(this.canvasSize["width"],this.canvasSize["height"])
    setTimeout(()=>{
      // 随机元素位置
      for (let e = 0; e < this.$refs.list.length; e++) {
        this.boxCoord(e)
      }
    },300)
    document.onkeyup = ()=>{
      if (window.event.keyCode === 32){
        this.musicPlay(this.musicRange(),4)
      }else if (window.event.keyCode === 65 || window.event.keyCode === 37){
        this.musicPlay(this.musicRange(),3)
      }else if (window.event.keyCode === 87 || window.event.keyCode === 38){
        this.musicPlay(this.musicRange(),2)
      } else if (window.event.keyCode === 68 || window.event.keyCode === 39){
        this.musicPlay(this.musicRange(),1)
      }
    }
    window.onresize = () =>{
      this.musicSize()
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
.musicClickHeight,
.musicClickMiddle,
.musicClickLow,
.musicClickChange{
  position: absolute;
  width: 60px;
  height: 60px;
  line-height: 60px;
  font-size: 24px;
  font-weight: bold;
  background: #ff876c;
  border-radius: 8px;
  transition: 0.3s;
}
.musicClickHeight{
  left: 20px;
  bottom: 180px;
}
.musicClickMiddle{
  left: 20px;
  bottom: 100px;
}
.musicClickLow{
  left: 20px;
  bottom: 20px;
}
.musicClickChange{
  bottom: 100px;
  right: 20px;
}
.tipsTitleBox{
  display: flex;
  justify-content: center;
  width: 100%;
  position: absolute;
  bottom: 30px;
  font-size: 26px;
  color: rgba(0, 0, 0, 0.61);
}
.tipsTitleBox span{
  margin: 0 15px 0 15px;
}
</style>