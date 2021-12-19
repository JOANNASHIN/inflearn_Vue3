<template>
    <h1>투두 리스트</h1>
    <TodoSimpleForm @add-todo="addTodo"></TodoSimpleForm>
    <TodoList :todos="todos"></TodoList>

    <!-- <div>카운트: {{ count }}</div>
    <div>더블카운트 computed: {{ doubleCount }}</div>
    <div>더블카운트 computed: {{ doubleCount }}</div>
    <div>더블카운트 computed: {{ doubleCount }}</div>
    <div>더블카운트 computed: {{ doubleCount }}</div>
    <div>더블카운트 method: {{ doubleCountMethod() }}</div>
    <div>더블카운트 method: {{ doubleCountMethod() }}</div>
    <button @click="count++;">카운트 숫자 증가</button> -->
</template>
<script>
import { ref, computed } from "vue";
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

        const addTodo = (todo) => {
            todos.value.push(todo)
        }

        const count = ref(1);

        //ref, reactive 에만 가능
        //값을 저장하고 있음 두번 위에서 사용한다고 해서 두번실행되지 않음
        const doubleCount = computed(() => {
            console.log("computed");
            return count.value * 2;
        })

        //인자값을 받아올 수 없음
        const doubleCountMethod = (() => {
            console.log("methods");
            return count.value * 2;
        })

        

        return {
            todos,
            addTodo,
            count,
            doubleCount,
            doubleCountMethod
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
