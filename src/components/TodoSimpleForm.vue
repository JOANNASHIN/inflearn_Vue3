<template>
   <form action="" @submit.prevent="addTodo">
        <input 
            type="text"
            v-model="todo" 
            placeholder="Type new todo"
        >
        <button type="submit">add</button>
    </form>
</template>

<script>
import { ref } from "vue";
export default {
    emits: ["add-todo"],
    setup(props, { emit }) {
        const todo = ref("");
        const hasError = ref(false);

        const addTodo = () => {
            if (todo.value === "") {
                hasError.value = true
            }
            else {
                emit("add-todo", {
                    id: Date.now(),
                    subject: todo.value,
                    completed: false,
                });

                hasError.value = false;
                todo.value = "";
            }
        }

        return {
            todo,
            hasError,
            addTodo
        }
    }
}
</script>

<style>
    
</style>
