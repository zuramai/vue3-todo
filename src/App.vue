<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <Header msg="Todo List" />
  <div class="container">
    <Table>
      <template v-slot:thead>
        <th>#</th>
        <th>Todo</th>
        <th>Status</th>
      </template>
      <template v-slot:tbody>
        <td colspan="3"   v-if="todos.length == 0">
          <Loading class='mx-auto'/>
        </td>
        <template v-else>
          <tr v-for="(todo,index) in todos" :key="index">
            <td>{{todo.id}}</td>
            <td>{{todo.title}}</td>
            <td>
              <span :class="{'badge':true, 'badge-success': todo.completed, 'badge-danger': !todo.completed}">{{todo.completed ? 'Completed' : 'Not Completed'}}</span>
            </td>
          </tr>
          <tr>
            <td></td>
            <td>
              <form method="post" @submit.prevent="addTodo">
                <input type="text" placeholder="Add new todo" v-model="newTodo">
                <button class="btn" type="submit">Add</button>
              </form>
            </td>
            <td></td>
          </tr>
        </template>
      </template>
    </Table>
  </div>
</template>

<script>
import axios from 'axios'
import Header from './components/Header.vue'
import Table from './components/Table.vue'
import Loading from './components/Loading.vue'
import { ref, onMounted } from 'vue'

export default {
  name: 'App',
  components: {
    Header, Table, Loading
  },
  setup(props) {
    let todos = ref([]);
    let newTodo = ref('')
    const addTodo = async () => {
      console.log(newTodo.value)
      let pushTodo = {
        id: todos.value.length+1,
        title: newTodo.value,
        completed: false
      }
      todos.value.push(pushTodo)
    }

    onMounted(() => {
      setTimeout(() => {
        axios.get('https://jsonplaceholder.typicode.com/todos')
          .then(res => {
            todos.value = res.data.splice(0,9);
          })
      }, 1000)
    })
    return {
      todos,
      newTodo,
      addTodo
    }
  }
}
</script>
