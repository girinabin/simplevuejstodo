<template>
    <div>
        <headers></headers>
        <add-todo @add-todo="addTodo"></add-todo>
        <todo :todos="todos" @del-todo="delTodo"></todo>
    </div>
</template>
<script>
import Todo from './components/Todo.vue'
import Header from './components/layout/header'
import AddTodo from './components/AddTodo'

export default {
    components:{
        'todo':Todo,
        'headers':Header,
        'add-todo':AddTodo
    },
    data(){
        return{
            todos:[]
        }
    },
    methods:{
        delTodo(id){
            this.todos = this.todos.filter(todo=>{
               return todo.id !== id
            })
        },
        addTodo(newTodo){
             this.todos.push(newTodo)
        }
    },
    created(){
        axios.get('https://jsonplaceholder.typicode.com/todos?_limit=7')
        .then(res =>this.todos=res.data)
        .catch(err => console.log(err))
    }
}
</script>
<style >
    *{
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }

    body{
        font-family: Arial, Helvetica, sans-serif;
        line-height: 1.4;
    }

    .btn{
        display: inline-block;
        border: none;
        background: #555;
        color: #fff;
        padding: 7px 20px;
        cursor: pointer;
    }

    .btn-hover{
        background: #666;
    }

</style>