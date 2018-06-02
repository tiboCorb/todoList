<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todo list</h1>
            <input type="text" class="new-todo" placeholder="Ajouter une tache" v-model='newTodo' @keyup.enter="addTodo">
        </header>
        <div class="main">
            <ul class="todo-list">
                <li v-for="todo in todos" :key="todo.name" class="todo " :class="{ completed :todo.completed}">
                    <div class="view">
                        <input class="toggle" type="checkbox" v-model="todo.completed">
                        <label>{{todo.name}}</label>
                    </div>
                </li>
            </ul>
        </div>
        <div>
            <footer class="footer">
                <span class="todo-count"><strong>{{remaning}}</strong></span>
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
      newTodo: ''
    }
  },
  methods: {
    addTodo () {
      this.todos.push({
        completed: false,
        name: this.newTodo
      })
      this.newTodo = ''
    }
  },
  computed: {
    remaning () {
      return this.todos.filter(todo => !todo.completed).length
    }
  }
}
</script>
<style src='./todos.css'></style>
