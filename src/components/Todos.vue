require("humanize-duration")

<template>
 <!-- représente le code HTML du template de vue = modèle-->
    <div class="todoapp">
        <header class="header">
            
            <h1>Todos</h1>

            <h3>{{ datedujour }}</h3>
            <input type="text" class="new-todo" placeholder="ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
        </header>
        <div class="conteneur" v-show="hasTodos">
            <div class="flex">
                <h2>A faire</h2>
                <ul class="todo-list">
                    <li class="todo"  v-for="(todo, index) in todos" :key=index >
                        <div class="view"  v-if="!todo.completed">
                            <input type="checkbox" v-model="todo.completed" @click="update(todo)">
                            <label >{{ todo.name }} le {{ todo.dateCreated.toLocaleString() }}</label>
                        </div>
                    </li>
                </ul>
            </div>

            <div class="flex">
                <h2>tâches terminées</h2>
                <ul class="todo-list-done" >
                    <li class="todo" v-for="todo in todos" :key=todo.name>
                        <div class="view" v-if="todo.completed">
                            <label>{{ todo.name }} le {{ todo.dateEnded.toLocaleString() }}</label>
                        </div>
                    </li>
                </ul>
            </div>

                <div class="flex">
                <h2>durée des tâches</h2>
                <ul class="todo-list-done" >
                    <li class="todo" v-for="todo in todos" :key=todo.name>
                        <div class="view" v-if="todo.completed">
                            <label >{{ todo.name }} : {{ humanizeDuree(todo.duree) }} </label>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
const humanizeDuration = require("humanize-duration");
// logique javascript
export default {
    
    name: 'Todos-item',  
     data(){
         return{
             todos:  [],
             newTodo: '',
         }
     },

     computed: {
          datedujour() {
            const currentDate = new Date().toLocaleString();
            return `nous sommes le ${currentDate}`;
          },
          hasTodos() {
              return this.todos.length > 0
          },
     },

     methods: {
         addTodo (){
             this.todos.push({
                 name: this.newTodo,
                 completed: false,
                 dateCreated: new Date()
             })
             this.newTodo =''
         },
         update (todo){
             this.todos.find(t => {
                 if(todo == t){
                todo.dateEnded= new Date()
                todo.duree =  todo.dateEnded.getTime() - todo.dateCreated.getTime()
                }
             }) 
        },

        humanizeDuree (duration){
            return humanizeDuration(duration, {language:'fr'})
        }
     }
}
</script>

<style src="./todos.css">
/* style du template */

</style>
