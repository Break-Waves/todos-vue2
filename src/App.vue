<template>
  <div id="root">
  <div class="todo-container">
    <div class="todo-wrap">
      <!-- 头部组件 -->
     <MyHeader @add="add" />
      <!-- 列表组件 -->
      <MyList  :todoThing="todoThing" />
      <!-- 底部组件 -->
      <MyFooter v-show="todoThing.length" :todoThing="todoThing" @clearDoneTodo="clearDoneTodo" @checkAll ="checkAll"/>
    </div>
  </div>
</div>

</template>

<script>
import pubsub from 'pubsub-js'
import {nanoid} from 'nanoid'
import MyHeader from './components/MyHeader.vue'
import MyList from './components/MyList.vue'
import MyFooter from './components/MyFooter.vue'

export default {
  name: 'App',
  data() {
        return {
            todoThing: JSON.parse(localStorage.getItem('todos'))||[]
        }
    },
  methods: {
    add(x){
      const newThing = {
        id: nanoid(),
        title: x,
        done: false
      }
      this.todoThing.unshift(newThing)
    },
    IsCheck(id){
      this.todoThing.forEach((t)=>{
          if(t.id == id){
            t.done = !t.done
          }return
      })
    },
    deleteTodo(_,id){//下划线表示占位，占据消息名参数的位
    this.todoThing = this.todoThing.filter((item)=>item.id != id)
    },
    checkAll(value){
    this.todoThing.forEach((item)=>{
      item.done = value
    })
    },
    editTodo(value,id){
      this.todoThing.forEach((t)=>{
        if(t.id == id){
          console.log(value);
          t.title = value
        }
      })
    },
    clearDoneTodo(){
    this.todoThing= this.todoThing.filter((item)=>item.done === false)
    }
  },
  mounted() {
    this.$bus.$on('IsCheck',this.IsCheck)
    this.pubId = pubsub.subscribe('deleteTodo', this.deleteTodo);
    this.$bus.$on('editTodo',this.editTodo)
  },
  beforeDestroy() {
    this.$bus.$off('IsCheck')
    this.$bus.$off('editTodo')
    pubsub.unsubscribe(this.pubId)
  },
  watch:{
    todoThing:{
      deep:true,
      handler(newValue){
        localStorage.setItem('todos',JSON.stringify(newValue))
      }
    }
   },
  components: {
    MyHeader,MyList,MyFooter
  }
}
</script>
<style>
    /*base*/
    body {
      background: #fff;
    }
    .btn {
      display: inline-block;
      padding: 4px 12px;
      margin-bottom: 0;
      font-size: 14px;
      line-height: 20px;
      text-align: center;
      vertical-align: middle;
      cursor: pointer;
      box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
      border-radius: 4px;
    }
     .btn-edit {
      color: #fff;
      background-color: skyblue;
      border: 1px solid rgb(95, 143, 162);
    }
    .btn-danger {
      color: #fff;
      background-color: #da4f49;
      border: 1px solid #bd362f;
    }
    .btn-danger:hover{
      color: #fff;
      background-color: #bd362f;
    }
    .btn-edit:hover{
      background-color: rgb(120, 184, 209);
    }
    .btn:focus {
      outline: none;
    }
    .todo-container {
      width: 600px;
      margin: 0 auto;
    }
    .todo-container .todo-wrap {
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 5px;
    }
</style>


