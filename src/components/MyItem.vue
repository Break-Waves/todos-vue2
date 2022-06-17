<template>
   <li >
          <label>
            <input type="checkbox"
                :checked="Thing.done"
                @change="handelCheck(Thing.id)"
            />
            <span v-show="!Thing.IsEdit">{{Thing.title}}</span>
            <input type="text" v-show="Thing.IsEdit" :value="Thing.title" @blur="editTodoBlur($event,Thing.id)">
          </label>
          <button class="btn btn-danger" @click="handelDelete(Thing.id)">删除</button>
          <button class="btn btn-edit" v-show="!Thing.IsEdit" @click="handelEdit(Thing)">编辑</button>
    </li>
</template>

<script>
import pubsub from 'pubsub-js'
export default {
    name:'MyItem',
    props:['Thing'],
    methods: {
      handelCheck(id){
        this.$bus.$emit('IsCheck',id)
      },
      handelDelete(id){
       if(confirm("你确定删除该事项吗？")){
        console.log(id);
        pubsub.publish('deleteTodo',id)
       }
      },
      handelEdit(Thing){
       if(Thing.hasOwnProperty('IsEdit')){
        Thing.IsEdit = true
       }else{
        this.$set(Thing,'IsEdit',true)
       }
      },
      editTodoBlur(e,id){
        // console.log(e.target.value);
        this.Thing.IsEdit = false
        this.$bus.$emit('editTodo',e.target.value,id)

      }
    },
}
</script>

<style>
    /*item*/
    li {
      list-style: none;
      height: 36px;
      line-height: 36px;
      padding: 0 5px;
      border-bottom: 1px solid #ddd;
    }

    li label {
      float: left;
      cursor: pointer;
    }

    li label li input {
      vertical-align: middle;
      margin-right: 6px;
      position: relative;
      top: -1px;
    }
  
    li .btn-danger , li .btn-edit {
      float: right;
      display: none;
      margin-top: 3px;
      margin-left: 5px;
    }

    li:before {
      content: initial;
    }

    li:last-child {
      border-bottom: none;
    }
    li:hover {
      background-color: #ccc;;
    }
    li:hover .btn-danger ,     li:hover .btn-edit {
      display: block;
    }
</style>