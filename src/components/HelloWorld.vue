<template>
    <p>{{name}}</p>
    <p>{{greeting("Joanna")}}</p>
    
    <p>id: {{test.id}}</p>

    <button @click="updateRefReactive()">change!</button>


<!-- 단방향 데이터바인딩 -->
    <input v-bind:value="databinding">
    <button v-on:click="updateData()">change data</button>

<!-- 양방향 데이터 바인딩 -->
    <input type="text" v-model="databinding">


    <input v-model="databinding">
    <p>현재 name은? {{databinding}}</p>

</template>

<script>
import { ref, reactive } from "vue";

export default {
    name: 'App',

    setup() {
        const name = ref("kossie coder");
        const test = reactive({
            id: 1
        })

        const greeting = (name) => {
            return `hello ${name}`
        }

        const updateRefReactive = () => {
            name.value = "namechanged";
            test.id = "2";
        }

        const databinding = ref("before");
        const updateData = () => {
            databinding.value = "after";

        }

        return {
            name,
            test,
            greeting,
            updateRefReactive,
            databinding,
            updateData
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
