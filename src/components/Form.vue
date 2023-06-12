<template>
    <header id="main-header" class="bg-warning text-black p-4 mb-3">
        <div class="container">
            <div class="row">
                <div class="col-md-6">
                    <h1 id="header-title">Планировщик</h1>
                </div>
                <div class="col-md-6 align-self-center">
                    <input v-model="searchQuery"
                        type="text"
                        class="form-control"
                        placeholder="Поиск по списку..."
                    />
                </div>
            </div>
        </div>
    </header>
    <form id="addForm" class="form-inline mb-3" @submit.prevent="addTodo">
        <input v-model="newTodo"
            type="text"
            class="form-control mr-2 flex-grow-1"
            placeholder="Добавить задачу"
            />
        <button class="btn btn-dark" @click="add">Добавить</button>
    </form>
    <hr />
    <h3 class="title mb-3">Список дел</h3>
    <ul class="list-group" >
        <li class="list-group-item" v-for="(todo, index) in filteredTodos" :key="index">
            {{ todo }}
            <button
                @click="removeTodo(index)"
                type="button"
                class="btn btn-light btn-sm float-right"
            >
                Удалить
            </button>
        </li>
    </ul>
</template>

<script>

export default {

    data() {
        return {
            todos: [],
            newTodo: '',
            searchQuery: '',
        }
    },

    methods: {

        addTodo() {
            if (this.newTodo.trim() !== '') {
                this.todos.push(this.newTodo);
                this.newTodo = '';
            }

            this.saveTodosToLocalStorage();
        },

        removeTodo(index) {
            this.todos.splice(index, 1);
            this.saveTodosToLocalStorage();
        },

        saveTodosToLocalStorage() {
            localStorage.setItem('todos', JSON.stringify(this.todos));
        },

        loadTodosFromLocalStorage() {
        const savedTodos = localStorage.getItem('todos');
            if (savedTodos) {
                this.todos = JSON.parse(savedTodos);
            }
        },
    },

    computed: {
    filteredTodos() {
      return this.todos.filter((todo) =>
        todo.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
  },

  mounted() {
    this.loadTodosFromLocalStorage();
  },
}

</script>

<style>
    
</style>