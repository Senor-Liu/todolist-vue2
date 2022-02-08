<template>
  <div class="TodoList">
     <section class="todoapp">
      <header class="header">
        <h1>todos</h1>
        <input type="checkbox" v-model="allDone">
        <input placeholder="What needs to be done?" v-model.trim="newTodoStr" @keyup.enter="addTodo()" class="new-todo">
      </header>
      <section class="main">
        <ul class="todo-list">
          <li v-for="(todo, id) in this.todoList" :key="id" class="todo">
            <todos :todo="todo" v-show="isShow(todo)" @removeTodo="removeTodo(todo)"></todos>
          </li>
        </ul>
      </section>
      <footer class="footer">
        <span class="todo-count">{{ remaining }} items left</span>
        <ul class="filters">
          <a @click="showChange('all')" :class="{active: currentPageName === 'all'}">All</a>
          <a @click="showChange('active')" :class="{active: currentPageName === 'active'}">Active</a>
          <a @click="showChange('completed')" :class="{active: currentPageName === 'completed'}">Completed</a>
        </ul>
        <span @click="removeCompleted" class="clear-completed">Clera completed</span>
      </footer>
    </section>
    <footer class="info">
      <p>Written by LJY</p>
    </footer>
  </div>
</template>

<script>
import todos from './todos.vue'
export default {
  components: { todos },
  name: 'TodoList',
  data () {
    return {
      todoList: [],
      newTodoStr: '',
      currentPageName: 'all'
    }
  },
  created () {
    this.initTodos()
  },
  watch: {
    todoList: {
      handler (todoList) {
        this.save(todoList)
      },
      deep: true // deep设置为true可深度检测对象属性变化
    }
  },
  computed: {
    remaining () {
      return this.todoList.filter(todo => !todo.completed).length
    },
    allDone: {
      get () {
        return this.remaining === 0
      },
      set (value) {
        this.todoList.forEach(function (todo) {
          todo.completed = value
        })
      }
    }
  },
  methods: {
    initTodos () {
      this.todoList = JSON.parse(localStorage.getItem('todos-vue2') || '[]')
    },
    addTodo () {
      if (!this.newTodoStr) {
        return
      }
      this.todoList.push({
        id: Date.now(),
        title: this.newTodoStr,
        completed: false
      })
      this.newTodoStr = ''
      this.save()
    },
    save () {
      localStorage.setItem('todos-vue2', JSON.stringify(this.todoList))
    },
    removeTodo (todo) {
      this.todoList.splice(this.todoList.indexOf(todo), 1)
    },
    removeCompleted () {
      this.todoList = this.todoList.filter(todo => !todo.completed)
    },
    isShow (todo) {
      return ((this.currentPageName === 'all') || (this.currentPageName === 'active' && !todo.completed) || (this.currentPageName === 'completed' && todo.completed))
    },
    showChange (type) {
      this.currentPageName = type
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
