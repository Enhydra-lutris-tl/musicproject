<template>
<div class="back">
  <input type="file" @change="imgInput" ref="imageInput" class="imageInput">
  <div class="imageInputButton"  ref="imageInputButton">
    <div class="newImageDelete" ref="newImageDelete" @click="newImageDeleted">清除背景</div>
    <i class="iconfont icon-icon-test2" @click="toImgInput"></i><span @click="toImgInput">修改背景</span></div>
  <img :src="imgSrc" alt="111111" id="img" ref="backImg" v-show="imgSrc" :style="{top:imgHeight*(topNumber/100)*-1 + 'px'}">
  <canvas id="canvasBack" :style="{position:'absolute',transition:'top 0.5s',top:imgHeight*(topNumber/100)*-1 + 'px',left:'0'}"></canvas>
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
      v-show="iconNumber"
      v-for="a in iconNumber"
      :key="a"
      class="list"
      ref="list"
  >
    <i class="iconfont icon-music musicIcon" ref="iconfont"></i>
  </div>
  <div class="musicClickLow" v-show="mobileShow" @click="musicClicked(1)">低</div>
  <div class="musicClickMiddle" v-show="mobileShow" @click="musicClicked(2)">中</div>
  <div class="musicClickHeight" v-show="mobileShow" @click="musicClicked(3)">高</div>
  <div class="musicClickChange" v-show="mobileShow" @click="musicClicked(4)">变</div>
<!--  todo相关-->
  <div class="todoBox" ref="todoBox">
  <i class="iconfont icon-icon-test1 todoCardChecked" @click="todoCardChecked" ref="iconfont"></i>
  <div class="todoListBox">
      <div class="todoTitleText">My Todo</div>
    <div class="operateBox">
      <div>{{myTodo.todo}}</div>
      <div class="buttonBox">
        <button type="button" @click="newTodo">添加</button>
        <button type="button" @click="clearData">重置</button>
      </div>
    </div>

    <ul>
      <li class="todoLiBox" v-for="todoList in todoListP" :key="todoList.id">
        <div class="todoCheckedBox" @click="todoChecked(todoList.id)" ref="todoCheckedBox"></div>
        <div class="todoText">{{todoList.todo}}</div>
        <div class="todoDelete" @click="todoDelete(todoList.id)">删除</div>
      </li>
    </ul>
  </div>
</div>
</div>
</template>

<script>
import {computed, reactive, ref} from "vue";

export default {
  name: "MusicList",
  data(){
    return{
      topNumber:0,
      imgHeight:'',
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
      imgSrc:localStorage.getItem('newImage'),
      imgRatio:''
    }
  },
  setup(){
    const todoCheckedBox = ref()
    const todoBox = ref()
    const iconfont =ref()
    const imageInputButton = ref()
    const newImageDelete = ref()

    let myTodo = reactive({
      id:'',
      checked:false,
      todo:'设置页面输入文本'
    })
    if (!localStorage.getItem('todoList')){
      let list = []
      localStorage.setItem('todoList',JSON.stringify(list))
    }
    let todoList = reactive({
      list:JSON.parse(localStorage.getItem('todoList')),
      cardChecked:true
    })
    //todolist排序
    let todoListP = computed(()=>{
      let todoListP1 = JSON.parse(JSON.stringify(todoList.list))
      return todoListP1.sort(function (a){
        return a.checked ? 1:-1
      })
    })


    // 卡片展开和收起
    function todoCardChecked(){
      todoList.cardChecked = !todoList.cardChecked
      iconfont.value.classList.remove('icon-icon-test')
      iconfont.value.classList.remove('icon-icon-test1')
      if (todoList.cardChecked === false){
        todoBox.value.classList.add('rotate')
        iconfont.value.classList.add('icon-icon-test')
        iconfont.value.classList.add('iconRotate')
        imageInputButton.value.style.right = '-90px'
        newImageDelete.value.style.top = '0'
        newImageDelete.value.style.color = 'transparent'
      }else if (todoList.cardChecked === true){
        todoBox.value.classList.remove('rotate')
        iconfont.value.classList.add('icon-icon-test1')
        iconfont.value.classList.remove('iconRotate')
        imageInputButton.value.style.right = '30px'
        newImageDelete.value.style.top = '-31px'
        newImageDelete.value.style.color = 'rgba(255, 255, 255, 0.5)'
      }
    }
    // 添加todo
    function newTodo(){
      myTodo.id = JSON.stringify(Math.random()*Math.random()*100)
      todoList.list.push(JSON.parse(JSON.stringify(myTodo)))
      myTodo.id = ''
      myTodo.todo = '设置页面输入文本'
      localStorage.setItem('todoList',JSON.stringify(todoList.list))

    }

    // 清除所有todo
    function clearData(){
      todoList.list.length = 0
      localStorage.setItem('todoList',JSON.stringify(todoList.list))

    }
    //删除一个todo
    function todoDelete(id){
      todoList.list = todoList.list.filter(a=>a.id !== id)
      localStorage.setItem('todoList',JSON.stringify(todoList.list))

    }
    //完成todo并改变提示圆颜色
    function todoChecked(id){
      for (let a = 0; a < todoList.list.length; a++){
        if (todoList.list[a].id === id){
          todoList.list[a].checked = !todoList.list[a].checked
          todoCheckedBox.value[a].style.background = todoList.list[a].checked? 'rgba(44,134,138,0.5)':'rgba(255, 255, 255, 0.6)'
        }
      }
    }
    return{
      todoList,
      myTodo,
      todoCheckedBox,
      todoListP,
      todoBox,
      iconfont,
      imageInputButton,
      newImageDelete,
      newTodo,
      clearData,
      todoDelete,
      todoChecked,
      todoCardChecked
    }
  },
  methods:{
    toImgInput(){
      this.$refs.imageInput.click()
    },
    // todo 更换壁纸后需要增加更新操作
    imgInput(){
      const file = this.$refs.imageInput.files[0]
      const fr = new FileReader()
      fr.onload = (e) => {
        this.imgSrc = e.target.result
        console.log(this.imgSrc)
        localStorage.setItem('newImage',this.imgSrc)
        this.imgWidthToHeight()
      }
      fr.readAsDataURL(file)
    },
    newImageDeleted(){
      localStorage.setItem('newImage','')
      this.imgSrc = localStorage.getItem('newImage')
    },
    // 随机选择一个音符
    musicRange(){
      return Math.floor(Math.random()*this.iconNumber.length)
    },
    // 调整画布尺寸
    drawBack(cx,cy){
      var canvas = document.getElementById('canvasBack')
      canvas.width = document.documentElement.clientWidth
      canvas.height = canvas.width*this.imgRatio

      this.$refs.backImg.width = canvas.width
      this.$refs.backImg.height = canvas.width*this.imgRatio
      // 可移动高度
      this.imgHeight = canvas.width*this.imgRatio - document.documentElement.clientHeight
      var ctx = canvas.getContext('2d')
      // 清除画布内容，防止拉伸
      ctx.clearRect(0,0,canvas.width,canvas.height)
      // var grd = ctx.createLinearGradient(0,0,canvas.width,canvas.height,)
      // grd.addColorStop(0,'#5FFBF1')
      // grd.addColorStop(1,'#D16BA5')
      // ctx.fillStyle=grd;
      // ctx.fillRect(0,0,canvas.width,canvas.height)
      var img = document.getElementById('img')
      ctx.drawImage(img,0,0,canvas.width,canvas.height)
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
    },
    // 获取图片宽高比
    imgWidthToHeight(){
      const img = new Image()
      img.src = this.imgSrc
      img.onload = ()=>{
        this.imgRatio = img.height/img.width
        this.$refs.backImg.height = document.documentElement.clientWidth * this.imgRatio
      }
    }
  },

  mounted() {
    if (!localStorage.getItem('newImage')){
      localStorage.setItem('newImage','')
    }
    this.imgWidthToHeight()
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
          //todo文本
          if (properties.newtodo){
            this.myTodo.todo = properties.newtodo.value
          }
          //图片位置
          if (properties.newimageheight){
            this.topNumber = properties.newimageheight.value
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
.imageInput{
  display: none;
}

.imageInputButton{
  position: absolute;
  display: flex;
  align-items: center;
  top: 14%;
  right: 30px;
  z-index: 3;
  height: 30px;
  width: 120px;
  border-radius: 4px 0 4px 4px;
  border: solid 1px rgba(255, 255, 255, 0.1);
  cursor: pointer;
  transition: 0.5s;
}
.imageInputButton:hover{
  background:rgba(255, 255, 255, 0.05);
}
.imageInputButton i:hover{
  background: rgb(147, 117, 149);
}
.imageInputButton span:hover{
  color: rgba(255, 255, 255, 0.8);
}
.newImageDelete{
  position: absolute;
  top: -32px;
  left: 29px;
  height: 30px;
  width: 90px;
  line-height: 30px;
  color: rgba(255, 255, 255, 0.5);
  font-size: 14px;
  border-radius: 4px 4px 0 0 ;
  border: solid 1px rgba(255, 255, 255, 0.1);
  transition: 0.8s;
}
.newImageDelete:hover{
  color: rgba(255, 255, 255, 0.8);
  background: rgba(245, 114, 114, 0.48);
}
.imageInputButton i{
  display: block;
  height: 30px;
  width: 30px;
  border-radius: 4px;
  background: rgb(208, 164, 212);
  font-size: 24px;
  line-height: 30px;
  transition: 0.7s;
}
.imageInputButton span{
  display: block;
  width: 90px;
  color: rgba(255, 255, 255, 0.5);
  font-size: 14px;
  transition: 0.5s;
}
#img{
  position: absolute;
  top: 0;
  left: 0;
  z-index: -2;
  transition:top 0.3s;
}
.list{
  position: absolute;
  height: 80px;
  width: 80px;
  top: calc(50% - 40px);
  left: calc(50% - 40px);
  line-height: 80px;
  transition: 0.5s;
  z-index: 1;
}
.musicIcon{
  display: block;
  position: absolute;
  height: 100%;
  width: 100%;
  font-size: 50px;
  transition: 0.5s;
  color: rgb(52, 52, 52);
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
  left: calc(50% - 500px);
  height: 50px;
  line-height: 50px;
  width: 1000px;
  border: rgba(0, 0, 0, 0.3) solid 2px;
  border-radius: 8px;
  z-index: 1;

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


/*todo相关*/

.todoBox{
  position: absolute;
  z-index: 3;
  top: 20%;
  right: 30px;
  min-width: 240px;
  max-width: 300px;
  transition: 0.5s;

}
.rotate{
  transform-origin: calc(100% - 19px) 15px;
  transform: rotate(-90deg);
  right:0;
}
.todoCardChecked{
  position: absolute;
  top: 0;
  right: 4px;
  display: block;
  height: 26px;
  width: 26px;
  border-radius: 50%;
  line-height: 26px;
  border: rgba(0,0,0,0.5) solid 2px;
  transition: 0.8s;
}
.iconRotate{
    transform: rotate(90deg);
}
.todoTitleText{
  height: 30px;
  margin-left: 25px;
  font-size: 24px;
  font-weight: bold;
  color: rgba(255, 255, 255, 0.5);
}
.operateBox{
  display: flex;
  justify-content: space-between;
  margin: 36px 0 10px 0;
}
.operateBox div:first-child{
  height: 30px;
  width: 60%;
  line-height: 30px;
  box-sizing: border-box;
  border-radius: 4px;
  font-size: 14px;
  color: rgba(0,0,0,0.3);
  border: rgba(255, 255, 255, 0.1) solid 1px;
  cursor: default;
}
.buttonBox{
  display: flex;
}
.buttonBox button{
  padding: 0 5px 0 5px;
  margin-left: 5px;
  border-radius: 4px;
  background: rgb(208, 164, 212);
  border: none;
  color: rgba(0,0,0,0.5);
  cursor: pointer;
}
.buttonBox button:hover{
  background: rgb(147, 117, 149);
  color: rgba(0,0,0,0.8)
}
.todoListBox ul{
  box-sizing: border-box;
}
.todoLiBox{
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 5px;
  border: rgba(255, 255, 255, 0.1) solid 1px;
  background: rgba(255, 255, 255, 0.05);
  padding: 5px 3px 5px 5px;
  border-radius: 4px;
  transition: 0.3s;
}
.todoCheckedBox{
  height: 20px;
  width: 20px;
  border:rgba(0,0,0,0.5) solid 2px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.6);
  cursor: pointer;
}
.todoText{
  font-size: 14px;
  color: rgba(0,0,0,0.6);
  margin: 0 5px 0 5px;
  cursor: default;
}
.todoDelete{
  height: 30px;
  width: 60px;
  border-radius: 4px;
  background: rgba(198, 70, 103, 0.5);
  line-height: 30px;
  font-size: 14px;
  color: rgba(0,0,0,0.5);
  cursor: pointer;
}
.todoDelete:hover{
  background: rgba(198, 70, 103, 0.7);
  color: rgba(0,0,0,0.8);
}
</style>