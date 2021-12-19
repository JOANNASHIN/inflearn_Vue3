<template>
    <h1>투두 리스트</h1>
    <input 
        type="text"
        v-model="searchText" 
        placeholder="search"
    >
    <TodoSimpleForm @add-todo="addTodo"></TodoSimpleForm>
    
    <div style="font-size: 14px; color: red;">{{ error }}</div>


    <div v-if="!filteredTodos.length">
        There is nothing to display
    </div>
    
    <TodoList 
        :todos="filteredTodos"
        @toggle-todo="toggleTodo"
        @delete-todo="deleteTodo"
    ></TodoList>
</template>
<script>
import { ref, computed } from "vue";
import axios from "axios";
import TodoSimpleForm from "./components/TodoSimpleForm.vue"
import TodoList from "./components/TodoList.vue"

export default {
    name: 'App',
    components: {
        TodoSimpleForm,
        TodoList
    },
    setup() {
        const todos = ref([]);
        const error = ref("");
        const dbUrl = "http://localhost:3000/todos";

        const getTodos = async () => {
            try {
                const res = await axios.get(dbUrl)
                todos.value = res.data;
            }
            catch(err) {
                console.error(err);
            }

        }

        const addTodo = async (todo) => {
            error.value ="";

            try {
                const res = await axios.post(dbUrl, {
                    subject: todo.subject,
                    completed: todo.completed
                })
    
                todos.value.push(res.data);
            }

            catch(err) {
                console.error(err);
                error.value = "Something went wrong.";
            }

            /*
                npm i axios
                >npm i -g json-server@0.15.0 

                json-server 키는 명령어: json-server --watch db.json

                [AXIOS & json-server]

                GET /posts
                GET /posts/1      >> 가져오기
                POST /posts       >> 추가
                PUT /posts/1
                PATCH /posts/1    >> 수정
                DELETE /posts/1   >> 삭제
            */

            // axios
            //     .post(dbUrl, {
            //         subject: todo.subject,
            //         completed: todo.completed
            //     }).then(res => {
            //         todos.value.push(res.data)

            //     }).catch(err => {
            //         console.error(err);
            //         error.value = "Something went wrong.";
            //     })
        }
        
        const deleteTodo = async (id) => {
            error.value ="";

            try {
                // const id = todos.value[index].id;
                await axios.delete(`${dbUrl}/${id}`);
                // todos.value.splice(index, 1);
            }
            catch(err) {
                console.error(err);
                error.value = "Something went wrong.";
            }
        }

        const toggleTodo = async (index) => {
            error.value ="";
            const id = todos.value[index].id;

            try {
                await axios.patch(`${dbUrl}/${id}`, {
                    completed: !todos.value[index].completed
                });

                todos.value[index].completed = !todos.value[index].completed;
            }
            catch(err) {
                console.error(err);
                error.value = "Something went wrong.";
            }
        }

        const searchText = ref("");

        const filteredTodos = computed(() => {
            if (searchText.value) {
                return todos.value.filter(todo => {
                    return todo.subject.includes(searchText.value);
                });
            }

            return todos.value;
        });

        getTodos();

        return {
            todos,
            error,
            getTodos,
            addTodo,
            deleteTodo,
            toggleTodo,
            searchText,
            filteredTodos
        }
    }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
