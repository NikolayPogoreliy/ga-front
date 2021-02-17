<template>
    <div>
        <router-link to="/">Home</router-link>
        <hr>
        <AddTodo
                @add-todo="addTodo"
        />
        <select v-model="filter">
            <option value="all">All</option>
            <option value="completed">Completed</option>
            <option value="not-completed">Not Completed</option>
        </select>
        <hr>
        <Loader v-if="loading"/>
        <TodoList
                v-else-if="filteredTodos.length"
                v-bind:todos="filteredTodos"
                @remove-todo="removeTodo"
        />
        <p v-else>No todos!</p>
    </div>
</template>

<script>
    import TodoList from '@/components/TodoList'
    import AddTodo from '@/components/AddTodo'
    import Loader from '@/components/Loader'

    export default {
        name: 'app',
        data() {
            return {
                todos: [],
                loading: true,
                filter: 'all'
            }
        },
        mounted() {
            fetch('http://0.0.0.0:8000/api/target/')
                .then(response => response.json())
                .then(json => {
                    setTimeout(() => {
                        console.log(json)
                        this.todos = json
                        this.loading = false
                    }, 1000)

                    console.log(this.todos)

                })
        },
        // watch: {
        //   filter(value) {
        //     console.log(value)
        //   }
        // },
        computed: {
            filteredTodos() {
                return this.todos
                // if (this.filter === 'all') {
                //   return this.todos
                // }
                //
                // if (this.filter === 'completed') {
                //   return this.todos.filter(t => t.completed)
                // }
                //
                // if (this.filter === 'not-completed') {
                //   return this.todos.filter(t => !t.completed)
                // }
            }
        },
        methods: {
            removeTodo(id) {
                this.todos = this.todos.filter(t => t.id !== id)
            },
            addTodo(todo) {
                this.todos.push(todo)
            }
        },
        components: {
            TodoList, AddTodo, Loader
        }
    }
</script>