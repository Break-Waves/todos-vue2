<template>
  <div class="todo-footer">
        <label>
          <input type="checkbox" v-model="isAll"/>
        </label>
        <span>
          <span>已完成{{doneTotal}}</span> / 全部{{todoThing.length}}
        </span>
        <button class="btn btn-danger" @click="clearTodo">清除已完成任务</button>
  </div>
</template>

<script>
export default {
    name:'MyFooter',
    props:['todoThing'],
    methods: {
      clearTodo(){
        this.$emit('clearDoneTodo')
      }
    },
    computed:{
      doneTotal(){
        return this.todoThing.reduce((current,next)=> current + next.done,0)
      },
      isAll:{
        get(){
          return this.doneTotal === this.todoThing.length && this.todoThing.length > 0
        },
        set(value){
          this.$emit('checkAll',value)
        }
      }
    }
}
</script>

<style>
    /*footer*/
    .todo-footer {
      height: 40px;
      line-height: 40px;
      padding-left: 6px;
      margin-top: 5px;
    }

    .todo-footer label {
      display: inline-block;
      margin-right: 20px;
      cursor: pointer;
    }

    .todo-footer label input {
      position: relative;
      top: -1px;
      vertical-align: middle;
      margin-right: 5px;
    }

    .todo-footer button {
      float: right;
      margin-top: 5px;
    }
</style>