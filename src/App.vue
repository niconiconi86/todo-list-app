<template>
  <div id="app">
    <div class="container">
      <h1 class="title">My Todo List</h1>
      <section class="input-section">
        <input
          type="text"
          class="input-field"
          placeholder="Add a new task..."
          v-model="inputTodo"
          @keyup.enter="addTodo"
        />
        <button class="add-btn" @click="addTodo">Add</button>
      </section>

      <ul class="todo-list">
        <li class="todo-item" v-for="(todo, index) in filteredTodoList" :key="index">
          <div v-if="!todo.editing">
            <span class="todo-text">{{ todo.text }}</span>
            <div class="actions">
              <button class="edit-btn" @click="editTodo(index)">Edit</button>
              <button class="delete-btn" @click="deleteTodo(todo)">Delete</button>
            </div>
          </div>

          <div v-else>
            <input
              type="text"
              class="input-field"
              v-model="todo.text"
              @keyup.enter="saveEdit(index)"
              @blur="saveEdit(index)"
            />
            <button class="save-btn" @click="saveEdit(index)">Save</button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      inputTodo: '',
      todoList: [],
    };
  },
  methods: {
    addTodo() {
      if (this.inputTodo.trim() !== '') {
        this.todoList.push({ text: this.inputTodo.trim(), editing: false });
        this.inputTodo = '';
      }
    },
    deleteTodo(todo) {
      const index = this.todoList.indexOf(todo);
      if (index > -1) {
        this.todoList.splice(index, 1);
      }
    },
    editTodo(index) {
      this.todoList[index].editing = true;
    },
    saveEdit(index) {
      if (this.todoList[index].text.trim() === '') {
        this.deleteTodo(this.todoList[index]);
      } else {
        this.todoList[index].editing = false;
      }
    },
  },
  computed: {
    filteredTodoList() {
      return this.todoList.filter((todo) => todo.text.trim() !== '');
    },
  },
};
</script>

<style>

body {
  margin: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f9f9f9;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.container {
  width: 100%;
  max-width: 500px;
  background: #ffffff;
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.title {
  text-align: center;
  font-size: 2rem;
  color: #333;
  margin-bottom: 1.5rem;
}


.input-section {
  display: flex;
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.input-field {
  flex: 1;
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
  outline: none;
  transition: border-color 0.3s ease;
}

.input-field:focus {
  border-color: #007bff;
}

.add-btn,
.save-btn {
  padding: 0.75rem 1.5rem;
  background-color: #fff000;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.add-btn:hover,
.save-btn:hover {
  background-color: #0056b3;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem;
  margin-bottom: 0.5rem;
  background: #f1f1f1;
  border-radius: 5px;
  font-size: 1rem;
}

.todo-text {
  flex: 1;
  word-break: break-word;
}

.actions {
  display: flex;
  gap: 0.5rem;
}

.edit-btn {
  padding: 0.5rem 1rem;
  background-color: #ffc107;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 0.875rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.edit-btn:hover {
  background-color: #e0a800;
}

.delete-btn {
  padding: 0.5rem 1rem;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 0.875rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.delete-btn:hover {
  background-color: #a71d2a;
}
</style>