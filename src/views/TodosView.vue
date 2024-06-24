<template>
  <div class="todo-app">
    <h1>Dimas Cell</h1>
    <form @submit.prevent="addTodo">
      <input type="text" v-model="newTodo" placeholder="Add Something!" />
      <button type="submit" class="primary">Add</button>
    </form>
    <table class="todo-table">
      <thead>
        <tr>
          <th width="50">Status</th>
          <th>Task</th>
          <th width="100">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(todo, index) in filteredTodos" :key="index">
          <td>
            <input type="checkbox" v-model="todo.done" />
          </td>
          <td>
            <span :class="{ 'done': todo.done }">{{ todo.text }}</span>
          </td>
          <td>
            <button @click="removeTodo(index)" class="danger">Remove</button>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="filter-section">
      <button @click="filterTodos('all')" class="filter-btn">All</button>
      <button @click="filterTodos('active')" class="filter-btn">Not Completed</button>
      <button @click="filterTodos('completed')" class="filter-btn">Completed</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      filter: 'all',
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length === 0) {
        return;
      }
      this.todos.push({
        text: this.newTodo,
        done: false,
      });
      this.newTodo = '';
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    filterTodos(filterType) {
      this.filter = filterType;
    },
  },
  computed: {
    filteredTodos() {
      switch (this.filter) {
        case 'active':
          return this.todos.filter(todo => !todo.done);
        case 'completed':
          return this.todos.filter(todo => todo.done);
        default:
          return this.todos;
      }
    },
  },
};
</script>

<style>
.todo-app {
  width: 800px;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  background-color: whitesmoke;
  color: rgb(2, 2, 9);
  margin-top: 50px;
}

.todo-app h1 {
  text-align: center;
  color: black;
  font-size: 36px;
  margin-bottom: 20px;
  font-weight: bold;
}

.todo-app form {
  display: flex;
  margin-bottom: 20px;
}

.todo-app input[type=text] {
  flex: 1;
  padding: 15px 20px;
  font-size: 1.2rem;
  border: 1px solid #ddd;
  border-radius: 5px;
  outline: none;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
}

.todo-app button[type=submit] {
  margin-left: 10px;
  padding: 10px 20px;
  font-size: 1.2rem;
  background-color: black;
  color: #fff;
}

.done {
  text-decoration: line-through;
  color: #747171;
}

.filter-section {
  text-align: center;
  margin-bottom: 20px;
}

.filter-btn {
  padding: 10px 20px;
  margin: 0 10px;
  font-size: 1rem;
  background-color: black;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.filter-btn:hover {
  background-color: gray;
}

.todo-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}

.todo-table th,
.todo-table td {
  padding: 10px;
  text-align: center;
  border: 1px solid black;
}

.todo-table th {
  background-color: black;
  color: #fff;
}

.todo-table td input[type="checkbox"] {
  margin-right: 10px;
}

.todo-table td .done {
  text-decoration: line-through;
}

.todo-table td .danger {
  padding: 5px 10px;
  font-size: 0.8rem;
  background-color: #dc3545;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.todo-table td .danger:hover {
  background-color: #c82333;
}
</style>
