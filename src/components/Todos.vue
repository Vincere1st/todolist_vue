<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todos</h1>
            <input type="checkbox" v-model="allDone" class="toggle">
            <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo"
                   @keyup.enter="addTodo">
        </header>
        <div class="main">
            <ul class="todo-list">
                <li class="todo" v-for="todo in filteredTodos" :class="{completed: todo.completed}">
                    <div class="view">
                        <input type="checkbox" v-model="todo.completed" class="toggle">
                        <label>{{ todo.name }}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
<!--                    <input type="text" @click="editTodo">-->
                </li>
            </ul>
        </div>
        <footer class="footer" v-show="hasTodos">
            <span class="todo-count"><strong>{{ remaining }}</strong>Tâche à faire</span>
            <ul class="filters">
                <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
                <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
                <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
            </ul>
            <button class="clear-completed" v-show="buttonDeleteDone" @click.prevent="deleteDone()">Supprimer les tâches finis</button>
        </footer>
    </section>
</template>

<script>
    export default {
        data() {
            return {
                todos: [{
                    name: 'Tache de test',
                    completed: false,
                }],
                newTodo: '',
                filter: 'all'
            }
        },
        methods: {
            addTodo() {
                this.todos.push({
                    completed: false,
                    name: this.newTodo,
                })
                this.newTodo = ''
            },
            deleteTodo(todoToDelete) {
                // const index = this.todos.indexOf(todoToDelete)
                // if (index !== -1) {
                //     this.todos.splice(index, 1)
                // }
                this.todos = this.todos.filter(todo => todo !== todoToDelete)
            },
            deleteDone () {
                return this.todos = this.todos.filter(todo => !todo.completed)
            }
        },
        computed: {
            allDone: {
                get() {
                    return this.remaining === 0
                },
                set(value) {
                    this.todos.forEach(todo => {
                        todo.completed = value
                    })
                }
            },
            remaining() {
                return this.todos.filter(todo => !todo.completed).length
                // Ligne du dessus identique à :
                //    this.todos.filter(function (todo)
                //    {
                //        return !todo.completed
                //    })
            },
            filteredTodos() {
                if (this.filter === 'todo') {
                    return this.todos.filter(todo => !todo.completed)
                } else if (this.filter === 'done') {
                    return this.todos.filter(todo => todo.completed)
                }
                return this.todos
            },
            hasTodos() {
                return this.todos.length > 0
            },
            buttonDeleteDone() {
                return this.todos.filter(todo => todo.completed).length // 0 is a falsy value so it's return false if there 0 completed task
            }
        }
    }
</script>

<style src="./todos.css">

</style>