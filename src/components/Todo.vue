<template>
<div>
  <div v-for="(todo, id) in data.todos" :key="id">
    <input type="checkbox" @change="checkTodo(id)" />
    {{todo.title}}
    <button @click="deleteTodo(id)">削除</button>
  </div>

  <button @click="deleteCheckedTodoos">チェック済みをすべて削除する</button>

  <div>
    <input type="text" v-model="data.inputValue" />
    <button @click="addTodo">追加</button>
  </div>
</div>
</template>

<script lang="ts">
import { defineComponent, reactive } from 'vue'
import { v4 } from 'uuid'

export default defineComponent({
  name: `Todo`,
  setup() {
    interface Data {
      inputValue: string
      todos: {
        [id: string]: { title: string, checked: boolean }
      }
    }

    const data = reactive<Data>({
      inputValue: ``,
      todos: {}
    })


    const addTodo = ():void => {
      data.todos = {
        ...data.todos, 
        [v4()]: {
          title: data.inputValue,
          checked: false
        }
      }
      data.inputValue = ``
    }

    const deleteTodo = (id: string):void => {
      const {[id]:_, ...deletedTodos} = data.todos
      data.todos = deletedTodos
    }

    const checkTodo = (id: string):void => {
      data.todos[id] = {...data.todos[id], checked: !data.todos[id].checked}
    }

    const deleteCheckedTodoos = ():void => {
      const unchecked = Object.fromEntries(
          Object.entries(data.todos).filter(([id, todo]) => {
            if(!todo.checked){
              return [id, todo]
            }
          })
        )

      data.todos = unchecked
    }

    return {
      data,
      addTodo,
      deleteTodo,
      checkTodo,
      deleteCheckedTodoos
    }
  },
})
</script>
