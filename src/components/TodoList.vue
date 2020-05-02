<template>
 <div>
    <input type="text" class="todo-input" placeholder="Write your todos" v-model="newTodo" @keyup.enter="addTodo">
    <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
        <div class="todo-item-overlap">
            <div v-if="!todo.editting" @dblclick="editTodo(todo)" class="todo-item-label">{{ todo.title }}</div>
            <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" v-focus @keyup.escape="cancelEdit(todo)">
        </div>
        <div class="remove-item" @click="removeTodo(index)">
            &times;
        </div>
    </div>
 </div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
      todos: [
          {
            'id': 1,
            'title': 'Finished Vue screen Test',
            'completed': false,
            'editting': false
            },
          {
            'id': 2,
            'title': 'Take',
            'completed': false,
            'editting': false
          }
      ]
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
    methods: {
      addTodo() {
          if(this.newTodo.trim().length === 0) {
            return
          }

          this.todos.push({
              id: this.idForTodo,
              title: this.newTodo,
              completed: false,
          })

          this.newTodo = ''
          this.idForTodo++
        },
        editTodo(todo) {
            todo.editting = true
        },
        doneEdit(todo) {
            todo.editting = false
        },
        removeTodo(index) {
            this.todos.splice(index, 1)
        }
    }
}
</script>

<style scoped>
.todo-input {
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom: 16px;
}

.todo-item {
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.3s;
}

.remove-item {
    cursor: pointer;
    margin-left: 14px;   
}

.remove-item:hover {
    color: black;
}

.todo-item-overlap {
    display: flex;
    align-items: center;
}

.todo-item-label {
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
}

.todo-item-edit {
    font-size: 24px;
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
}
</style>
