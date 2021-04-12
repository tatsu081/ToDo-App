<template>
  <div class="container">
<!--    {{ todos }}-->
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <span v-if="todo.created">
          <input
            type="checkbox"
            v-bind:checked="todo.done"
            @change="toggle(todo)">
          <span :class="{ done: todo.done }">
           {{ todo.name }} {{ todo.created.toDate() | deteFilter }}
          </span>
          <button @click="remove(todo.id)">×</button>
        </span>
      </li>
    </ul>
    <div class="form">
      <form v-on:submit.prevent="add">
        <input v-model="name">
        <button>Add</button>
      </form>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
export default {
  name: "todos",
  data: function (){
    return {
      name:'',
      done: false
    }
  },
  created: function () {
    this.$store.dispatch('todos/init')
  },
  methods: {
    add(){
      this.$store.dispatch('todos/add', this.name)
      this.name = ''
    },
    remove(id){
      this.$store.dispatch('todos/remove',id)
    },
    toggle(todo){
      this.$store.dispatch('todos/toggle', todo)
    }
  },
  computed: {
    todos(){
      // return this.$store.state.todos.todos
      return this.$store.getters['todos/orderdTodos']
    }
  },
  filters: {
    deteFilter: function (date){
      return moment(date).format('YYYY/MM/DD HH:mm:ss')
    }
  }
}
</script>

<style scoped>
.container{
  padding: 0;
  width: 50%;
  margin: 100px auto 0;
}
ul{
  padding: 0;
}
ul li{
  list-style: none;
}
li > span > span.done{
  text-decoration: line-through;
}
</style>
