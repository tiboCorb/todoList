<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todo list</h1>
            <input type="text" class="new-todo" placeholder="Ajouter une tache" v-model='newTodo' @keyup.enter="addTodo">
        </header>
        <div class="main">
          <input type="checkbox" class='toggle-all' v-model="allDone">
            <ul class="todo-list">
                <li v-for="todo in filtered" :key="getIndex(todo)" class="todo " :class="{ completed :todo.completed, editing : todo === editing}">
                    <div class="view">
                        <input class="toggle" type="checkbox" v-model="todo.completed">
                        <label @dblclick="editTodo(todo)">{{todo.name}}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
                      <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" @blur="doneEdit" @keyup.esc="cancelEdit" v-focus="todo === editing">
                </li>
            </ul>
        </div>
        <div>
            <footer class="footer" v-show="todos.length > 0">
                <span class="todo-count"><strong>{{remaning}}</strong> tâche à faire </span>
                <ul class="filters">
                  <li><a href="%23" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
                  <li><a href="%23" :class="{selected: filter === 'todo'}" @click.prevent="filter ='todo'">A faire</a></li>
                  <li><a href="%23" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
                </ul>
                <button class="clear-completed" v-show="hasDone" @click.prevent="deleteDone">Clean</button>
            </footer>
        </div>
    </section>
</template>

<script>
export default {
  data () {
    return {
      todos: [{
        name: 'Tache test',
        completed: false
      }],
      newTodo: '',
      filter: 'all',
      editing: null,
      oldTodo: ''
    }
  },
  methods: {
    cancelEdit (todo) {
      this.editing.name = this.oldTodo
      this.editing = null
    },
    getIndex (todo) {
      return this.todos.indexOf(todo)
    },
    doneEdit () {
      this.editing = null
    },
    deleteTodo (obj) {
      this.todos = this.todos.filter(todo => todo !== obj)
    },
    deleteDone () {
      this.todos = this.todos.filter(todo => !todo.completed)
    },
    editTodo (todo) {
      this.oldTodo = todo.name
      this.editing = todo
    },
    addTodo () {
      this.todos.push({
        completed: false,
        name: this.newTodo
      })
      this.newTodo = ''
    }
  },
  computed: {
    hasDone () {
      return this.todos.filter(todo => todo.completed).length > 0
    },
    allDone: {
      get () {
        return this.remaning === 0
      },
      set (value) {
        this.todos.forEach(todo => {
          todo.completed = value
        })
      }
    },
    remaning () {
      return this.todos.filter(todo => !todo.completed).length
    },
    filtered () {
      switch (this.filter) {
        case 'all':
          return this.todos
        case 'todo':
          return this.todos.filter(todo => !todo.completed)
        case 'done':
          return this.todos.filter(todo => todo.completed)
      }
    }
  },
  directives: {
    focus (el, value) {
      if (value) {
        el.focus()
      }
    }
  }
}
</script>
<style src='./todos.css'></style>
