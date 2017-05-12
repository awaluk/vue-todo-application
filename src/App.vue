<template>
    <div class="container">
        <div class="col-md-6 col-md-offset-3">
            <h1>Vue TODO App</h1>
            <table class="table table-striped table-bordered">
                <tbody>
                    <tr>
                        <th>Task</th>
                        <th>Date</th>
                        <th>Finished?</th>
                        <th></th>
                    </tr>
                    <tr v-for="(todo, index) in todos">
                        <td>{{ todo.title }}</td>
                        <td>{{ todo.date }}</td>
                        <td :data-index="index" v-on:click="modify">
                            <button class="btn btn-success btn-xs" v-if="todo.finished === true">Yes</button>
                            <button class="btn btn-danger btn-xs" v-else>No</button>
                        </td>
                        <td><button v-on:click="remove($event)" class="btn btn-danger btn-xs" :data-index="index">x</button></td>
                    </tr>
                    <tr>
                        <td><input type="text" id="title" v-model="title" class="form-control"></td>
                        <td><input type="date" v-model="date" class="form-control"></td>
                        <td><input type="checkbox" v-model="finished"></td>
                        <td><button class="btn btn-default" v-on:click="add">+</button></td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
    'use strict';

    export default {
        data() {
            return {
                todos: JSON.parse(localStorage.getItem('todos', )) || [],
                title: '',
                date: '',
                finished: false
            }
        },
        methods: {
            add: function() {
                if (this.title.length > 0) {
                    this.todos.push({
                        title: this.title,
                        date: this.date,
                        finished: !!this.finished
                    });
                    this.saveToStorage();
                    this.title = '';
                    this.finished = false;
                }
            },
            modify: function(event) {
                let index = event.currentTarget.dataset.index;
                this.todos[index].finished = !this.todos[index].finished;
                this.saveToStorage();
            },
            remove: function(event) {
                let index = event.currentTarget.dataset.index;
                this.todos.splice(index, 1);
                this.saveToStorage();
            },
            saveToStorage: function() {
                localStorage.setItem('todos', JSON.stringify(this.todos));
            }
        }
    }
</script>
