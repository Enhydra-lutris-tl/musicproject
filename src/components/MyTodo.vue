<template>
<div class="todoBox" ref="todoBox">
  <i class="iconfont icon-icon-test1 todoCardChecked" @click="todoCardChecked" ref="iconfont"></i>
  <div class="todoListBox">
      <div class="todoTitleText">My Todo</div>
    <div class="operateBox">
      <div>{{myTodo.todo}}</div>
      <div class="buttonBox">
        <button type="button" @click="newTodo">添加</button>
        <button type="button" @dblclick="clearData">重置</button>
      </div>

    </div>

    <ul>
      <li class="todoLiBox" v-for="todoList in todoListP" :key="todoList.id">
        <div class="todoCheckedBox" @click="todoChecked(todoList.id)" ref="todoCheckedBox"></div>
        <div class="todoText">{{todoList.todo}}</div>
        <div class="todoDelete" @dblclick="todoDelete(todoList.id)">删除</div>
      </li>
    </ul>
  </div>


</div>
</template>

<script>
import {ref,reactive,computed} from 'vue'
export default {
  name: "MyTodo",
  setup(){
    const todoCheckedBox = ref()
    const todoBox = ref()
    const iconfont =ref()

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

      }else if (todoList.cardChecked === true){
        todoBox.value.classList.remove('rotate')
        iconfont.value.classList.add('icon-icon-test1')
        iconfont.value.classList.remove('iconRotate')
      }
    }
    // 添加todo
    function newTodo(){
      myTodo.id = JSON.stringify(Math.random()*Math.random()*100)
      todoList.list.push(JSON.parse(JSON.stringify(myTodo)))
      myTodo.id = ''
      myTodo.todo = '设置页面输入文本'
      localStorage.setItem('todoList',JSON.stringify(todoList.list))
      console.log(todoList.list)
    }

    // 清除所有todo
    function clearData(){
      todoList.list.length = 0
      localStorage.setItem('todoList',JSON.stringify(todoList.list))
      console.log(todoList.list)
    }
    //删除一个todo
    function todoDelete(id){
      todoList.list = todoList.list.filter(a=>a.id !== id)
      localStorage.setItem('todoList',JSON.stringify(todoList.list))
      console.log(todoList.list)
    }
    //完成todo并改变提示圆颜色
    function todoChecked(id){
      for (let a = 0; a < todoList.list.length; a++){
        if (todoList.list[a].id === id){
          todoList.list[a].checked = !todoList.list[a].checked
          console.log(todoList.list[a].checked)
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
      newTodo,
      clearData,
      todoDelete,
      todoChecked,
      todoCardChecked
    }
  },
  mounted() {
     window.wallpaperPropertyListener = {
        applyUserProperties: (properties) =>{
          if (properties.ceshishuju){
            this.myTodo = properties.ceshishuju.value
          }
        }
     }

  }
}
</script>

<style scoped>
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