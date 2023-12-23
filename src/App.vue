<script setup>
import { ref, onMounted, computed, watch } from 'vue';

const todos = ref([]);
const name = ref('');
const input_content = ref('');
const input_category = ref(null);

const finishedTodos = ref(0);

const todosDone = computed(() => {
  return todos.value.filter(todo => todo.done).length;
});

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt;
}));

const addTodo = () => {
  if (input_content.value.trim() === '') {
    return;
  }

  todos.value.push({
    content: input_content.value,
    done: false,
    createdAt: new Date().getTime(),
  });
  input_content.value = ''; // Clear input field
};

const lineThrough = (todo) => {
  todo.done = !todo.done;
  updateFinishedTodos();
};

const deleteTodo = (todo) => {
  const index = todos.value.indexOf(todo);
  if (index !== -1) {
    todos.value.splice(index, 1);
    // updateFinishedTodos();
    console.log(finishedTodos)
  }
};

const updateFinishedTodos = () => {
  finishedTodos.value++;
};

watch(name, (newVal) => {
  localStorage.setItem('name', newVal);
});

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal));
}, {
  deep: true,
});

onMounted(() => {
  name.value = localStorage.getItem('name') || '';
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up,
      </h2>
      <input type="text" placeholder="name here" class="nameHolder" v-model="name" />
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>
      <form action="" @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input type="text" class="todo-input" placeholder="e.g. make a video!" v-model="input_content">
        <input type="submit" value="add todo" class="addTodo">
      </form>
    </section>

    <section class="todo-list">
      <h3>TODO LIST!</h3>
      <p>Finished Todos: {{ finishedTodos }}</p>
      <div class="list">
        <div v-for="todo in todos_asc" class="todo-item">
          <span :class="todo.done ? 'done' : ''">{{ todo.content }}</span>
          <button class="deleteButton" @click="lineThrough(todo)">mark todo finished</button>
          <button class="removeTodo" @click="deleteTodo(todo)">Delete Todo</button>
        </div>
      </div>
    </section>
  </main>
</template>


<style scoped>
* {
  font-family: monospace;
  background-color: rgb(20, 136, 107);
  color: rgb(25, 22, 22);

}
.nameHolder{
  display: flex;
  flex-direction: row;
  justify-content: center;
}
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex-wrap: wrap;
  justify-content: center;
  height: 100vh;
  font-family: monospace;
 
  border: 1px solid black
}

.greeting, .create-todo, .todo-list {
  width: 100%;
  max-width: 350px;
  text-align: center;
  margin-bottom: 20px;
  
}

.nameHolder {
  font-size: 1.2rem;
  /* border: 1px solid white; */
  width: 100%;
  padding: 5px;
  
}

.todo-input {
  width: 100%;
  padding: 10px;
  margin-top: 10px;
  background-color: whitesmoke;
  border-radius: 5px 5px 5px 5px;
}

.todo-item {
  border: 1px solid #ccc;
  padding: 10px;
  margin: 5px 0;
}

.done {
  text-decoration: line-through;
}

.addTodo {
  width: 100px;
  height: 35px;
  padding: 10px;
  border-top: 10px;
  background-color: rgb(163, 191, 224);
}

.addTodo:hover {
  transform: scale(1.1);
}
.deleteButton{
  display: block;
  justify-content: space-around;
  align-items: end;
  background-color: rgb(163, 191, 224);
}

/* .todo-list {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
} */

.removeTodo {
  display: flex;
  background-color: rgb(163, 191, 224);
}

.greeting {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row; /* Display elements side by side */
  margin-bottom: 20px;
}

.greeting-content {
  display: flex;
  flex-direction: column; /* Display "What's up" and input vertically */
  align-items: center;
}

.title {
  margin-right: 10px; /* Add some spacing between "What's up" and input */
}

.nameHolder {
  font-size: 1.2rem;
  border: 1px solid white;
  width: 100%;
  padding: 5px;
}


.todoList {
  border: 1px solid grey;
  border-radius: 5px 5px 5px 5px;
}

.list{
  border-radius: 15px 15px 15px 15px;
}


</style>
