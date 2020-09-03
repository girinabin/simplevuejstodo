<template>
    <div>
        <form @submit.prevent="saveData">
            <div class="input-group mb-3 input-group-lg">
                <input
                    @keydown="form.errors.clear('title')"
                    :class="{ 'is-invalid': form.errors.has('title') }"
                    v-model="form.title"
                    type="text"
                    class="form-control"
                    placeholder="Todo Title"
                    aria-label="Recipient's username"
                    aria-describedby="basic-addon2"
                />
                <div class="input-group-append">
                    <button class="btn btn-success" type="submit">
                        Add Todo
                    </button>
                </div>
            </div>
            <span
                class="text-danger pt-3 errorclass"
                v-if="form.errors.has('title')"
                v-text="form.errors.get('title')"
            ></span>
        </form>

        <div class="">
            <div class="w-100 card" v-for="(todo, index) in todos" :key="index">
                <input @click="toggleTodo(todo)"  type="checkbox" name="completed" id="" :checked="todo.completed==1" />
                <p>{{ todo.title }}</p>
                <div>
                    <a href="" class="float-right">Edit </a>
                    <a href="" class="float-right">Delete </a>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            todos: [],
            form: new Form({
                title: ""
            })
        };
    },
    mounted() {
        this.getTodos();
    },
    methods: {
        saveData() {
            let data = new FormData();
            data.append("title", this.form.title);
            axios
                .post("api/todo", data)
                .then(res => {
                    this.form.reset();
                    this.getTodos();
                })
                .catch(err => {
                    this.form.errors.record(err.response.data.errors);
                });
        },
        getTodos() {
            axios.get("api/todo").then(res => {
                this.todos = res.data.todos;
            });
        },
        toggleTodo(todo){
            todo.completed = !todo.completed
            let data = new FormData()
            data.append('_method','PATCH')
            if(todo.completed == true){
                data.append("completed",1)
            }else{
                data.append("completed",0)
            }
            axios.post(`api/todo/${todo.id}`,data)
            
        }
    }
};
</script>

<style scoped>
.errorclass {
    font-size: 24px;
    padding: 3px 0;
}
</style>
