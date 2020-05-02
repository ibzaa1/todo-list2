<template>
 <div>
    <input type="text" class="todo-input" placeholder="Add todos" v-model="newTodo" @keyup.enter="addTodo">
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
    <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">
        <div class="todo-item-overlap">
            <input type="checkbox" v-model="todo.completed">
            <div v-if="!todo.editting" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed }">{{ todo.title }}</div>
            <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" v-focus @keyup.esc="cancelEdit(todo)">
        </div>
        <div class="remove-item" @click="removeTodo(index)">
            &times;
        </div>
    </div>
    </transition-group>

    <div class="extra-container">
    <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAll"> Check All</label></div>
    <div>{{ remaining }} items left</div>
    </div>

    <div class="extra-container">
        <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
        <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Not Completed</button>
        <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
        </div>

        <div>
            <transition name="fade">
            <button class="clear" v-if="showClearCompleted" @click="clearCompleted">Clear Completed</button>
            </transition>
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
      beforeEdit: '',
      filter: 'all',
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
  computed: {
    remaining() {
        return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining() {
        return this.remaining != 0
    },
    todosFiltered() {
      if (this.filter == 'all') {
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }
    },
    showClearCompleted () {
        return this.todos.filter(todo => todo.completed).length > 0 
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
            this.beforeEdit = todo.title
            todo.editting = true
        },
        doneEdit(todo) {
            if(todo.title.trim().length === 0) {
            todo.title = this.beforeEdit
          }
            todo.editting = false
        },
        cancelEdit(todo) {
            todo.editting = false,
            todo.title = this.beforeEdit
        },
        removeTodo(index) {
            this.todos.splice(index, 1)
        },
        checkAll() {
          this.todos.forEach((todo) => todo.completed = event.target.checked)
        },
        clearCompleted() {
          this.todos = this.todos.filter(todo => !todo.completed)
        }
    }
}
</script>

<style scoped>
 @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

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
    animation-duration: 1s;
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

.completed {
    text-decoration: line-through;
    color: grey;
}

.extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 14px;
    margin-bottom: 14px;
}

button {
    border: none;
    box-shadow: none;
    font-size: 14px;
    color: white;
    background-color: #35495e;
    appearance: none;
    padding: 10px;
    border-radius: 5px;
    font-size: 15px;
    cursor: pointer;
}

.active {
    background: #41b883;
}

.clear {
  background-color: #c62a2a;
}

.fade-enter-active, .fade-leave-active {
    transition: opacity .2s;
}
.fade-enter, .fade-leave-to {
    opacity: 0;
}
</style>
