<template>
<div class="back">

  <canvas id="canvasBack"></canvas>
  <div class="dateList">
    <div class="nowTime">{{nowDate['nowTime']}}</div>
    <div class="nowDate">{{nowDate['nowDate']}}</div>
  </div>
  <div class="text" :style="{color:textColor}">{{text}}</div>

  <div class="tipsTitleBox" v-show="tipShow">
    <span>点左上,高音</span>
    <span>点右上,中音</span>
    <span>点左下,低音</span>
    <span>点右下,变音</span>
  </div>
  <div class="mouseClick ClickLeftTop" @click="musicClicked(3)"></div>
  <div class="mouseClick ClickRightTop" @click="musicClicked(2)"></div>
  <div class="mouseClick ClickLeftBottom" @click="musicClicked(1)"></div>
  <div class="mouseClick ClickRightBottom" @click="musicClicked(4)"></div>

  <div class="mouseTipsBox" v-show="tipShow">
    <div class="mouseTips tipsLeftTop" @click="musicClicked(3)"></div>
    <div class="mouseTips tipsRightTop" @click="musicClicked(2)"></div>
    <div class="mouseTips tipsLeftBottom" @click="musicClicked(1)"></div>
    <div class="mouseTips tipsRightBottom" @click="musicClicked(4)"></div>
  </div>
  <div
      v-for="a in iconNumber"
      :key="a"
      class="list"
      ref="list"
  >
    <i class="iconfont icon-music" ref="iconfont"></i>
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
      tipShow:false,
      nowDate: {nowTime:'',nowDate:''},
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
      text:'new Text|一段文本',
      textColor:'rgba(0,0,0,0.5)',
      iconNumber:[0,1,2,3,4],
    }
  },
  methods:{
    // 随机选择一个音符
    musicRange(){
      return Math.floor(Math.random()*this.iconNumber.length)
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
        if(bodyHeight - boxTop <= 120 ){
          boxTop = bodyHeight - 120
        }
      }
      if (boxTop <= 120){
          boxTop = 120
      }
      if (boxLeft <= 120){
        boxLeft = 120
      }else if(bodyWidth - boxLeft <= 120 ){
        boxLeft = bodyWidth - 120
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
    setInterval(()=>{
      this.nowDate["nowTime"] = new Date().toLocaleTimeString()
      this.nowDate["nowDate"] = new Date().toLocaleDateString()
    },1000)
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

    //壁纸相关
    window.wallpaperPropertyListener = {
        applyUserProperties: (properties) =>{
          //获取输入的文本
          if (properties.newtext) {
             this.text = properties.newtext.value
          }
          // 获取文本颜色
          if (properties.newtextcolor) {
            var newtextcolors = properties.newtextcolor.value.split(' ')
            newtextcolors = newtextcolors.map(function (c) {
              return Math.ceil(c*255)
            })
            this.textColor = `rgb(${newtextcolors})`
          }
            //选择音符数量
          if (properties.newiconnumber) {
            if (this.iconNumber.length < properties.newiconnumber.value){
              // 差值
              let a = properties.newiconnumber.value - this.iconNumber.length
              for (let b = 0; b < a; b++){
                this.iconNumber.push(this.iconNumber.length)
              }
            }else if (this.iconNumber.length > properties.newiconnumber.value){
              this.iconNumber = this.iconNumber.filter(a=>a < properties.newiconnumber.value)
            }
          }
          //互动提示开关
          if (properties.newtipchecked) {
            this.tipShow = properties.newtipchecked.value
          }
        },
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
  top: 30px;
  font-size: 26px;
  color: rgba(0, 0, 0, 0.1);
}
.tipsTitleBox span{
  margin: 0 15px 0 15px;
}
.mouseClick{
  position: absolute;
  height: 50%;
  width: 50%;
  background: transparent;
  font-size: 120px;
  font-weight: bold;
  line-height: 400px;
  color: rgba(0, 0, 0, 0.05);
  z-index: 1;
}
.ClickLeftTop{
  top: 0;
  left: 0;
}
.ClickRightTop{
  top: 0;
  right: 0;
}
.ClickRightBottom{
  bottom: 0;
  right: 0;
}
.ClickLeftBottom{
  bottom: 0;
  left: 0;
}
.dateList{
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  z-index: 2;
  top: 80px;
  left: calc(50% - 250px);
  height: 120px;
  width: 500px;
  border-radius: 8px;
  color: rgba(255, 255, 255, 0.87);
}
.nowTime{
  margin-top: 15px;
  font-size: 60px;
  font-weight: bold;

}
.nowDate{
  font-weight: bold;
  font-size: 18px;
  margin-bottom: 15px;
}
.text{
  position: absolute;
  top: calc(50% - 25px);
  left: calc(50% - 600px);
  height: 50px;
  line-height: 50px;
  width: 1200px;
  border: rgba(0, 0, 0, 0.3) solid 2px;
  border-radius: 8px;

}
.mouseTips{
  position: absolute;
  height: 30px;
  width: 30px;
  box-sizing: border-box;
}
.tipsLeftTop{
  top: calc(50% - 30px);
  left: calc(50% - 30px);
  border-right: rgba(0, 0, 0, 0.1) solid 1px;
  border-bottom: rgba(0, 0, 0, 0.1) solid 1px;
}
.tipsRightTop{
  top: calc(50% - 30px);
  right: calc(50% - 30px);
  border-bottom: rgba(0, 0, 0, 0.1) solid 1px;
  border-left: rgba(0, 0, 0, 0.1) solid 1px;
}
.tipsLeftBottom{
  bottom: calc(50% - 30px);
  left: calc(50% - 30px);
  border-top: rgba(0, 0, 0, 0.1) solid 1px;
  border-right: rgba(0, 0, 0, 0.1) solid 1px;
}
.tipsRightBottom{
  bottom: calc(50% - 30px);
  right: calc(50% - 30px);
  border-left: rgba(0, 0, 0, 0.1) solid 1px;
  border-top: rgba(0, 0, 0, 0.1) solid 1px;
}
</style>