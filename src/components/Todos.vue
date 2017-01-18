<template>

    <div>
        <div v-show="!authorized">
            <md-button class="md-raised md-warn">Warn</md-button>
        </div>

        <ul>
            <li v-for="(todo, index) in todos"> {{ todo.name }}</li>
        </ul>
    </div>
</template>
<style>
</style>
<script>
// localStorage persistence
var STORAGE_KEY = 'todosstorage'
    var todoStorage = {
      fetch: function () {
        var todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
        todos.forEach(function (todo, index) {
          todo.id = index
        })
        todoStorage.uid = todos.length
        return todos
      },
      save: function (todos) {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
      }
    }

export default{
  data () {
    return {
      todos: []
    }
  },
  created () {
    this.fetchData()
  },
  methods: {
    fetchData: function () {
      return this.fetchPage(1)
    },
    fetchPage: function (page) {
      this.$http.get('http://todos.dev:8080/api/v1/task?page=' + page).then((response) => {
        console.log(response.data)
        this.todos = response.data.data
      }, (response) => {
        console.log(response.data)
      })
    }

  }
}
</script>