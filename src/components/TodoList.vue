<template>
  
    <div class="card" style="opacity: 0.5">
        <div class="card-body" 
            v-for="(todo, index) of todos" 
            :key="index"
        >
            <div class="form-check flex-grow-1">
                <input 
                    class="form-check-input"
                    type="checkbox"
                    :value="todo.completed"
                    @change="toggleTodo(index)"
                    :checked="todo.completed"
                >
                <label
                    class="form-check-label"    
                    :class="{todo : todo.completed}"
                >
                    {{todo.subject}}
                </label>
                <button
                    class="btn btn-danger btn-sm"
                    @click="openModal(todo.id)"
                >delete</button>
            </div>
        </div>
    </div>

    <teleport to="#modal">
        <ModalComponent
            v-if="showModal"
            @close="closeModal"
            @delete="deleteTodo"
        >
        </ModalComponent>

        <DeleteModal></DeleteModal>
    </teleport>

    <teleport to="#kossie">
        <div>kossie coder</div>
    </teleport>
</template>

<script>
import { ref } from "vue";
import ModalComponent from "@/components/DeleteModal.vue";
import { useToast } from "@/composables/toast";

export default {
    //String, Number, Boolean, Array, Object, Function, Promise
    //중요한것: props는 부모컴포넌트 > 자식컴포넌트로만 전달! 자식컴포넌트에서 직접적으로 바꾸지 않기
    components: {
        ModalComponent,
    },
    props: {
        todos: {
            type: Array,
            required: true
        }
    },

    setup(props, { emit }) {
        const {
            toastMessage,
            toastAlertType,
            showToast,
            triggerToast
        } = useToast();

        const showModal = ref(false);
        const todoDeleteId = ref(null);

        const openModal = (id) => {
            todoDeleteId.value = id;
            showModal.value = true;
        }

        const closeModal = () => {
            todoDeleteId.value = null;
            showModal.value = false;
        }

        const toggleTodo = (index) => {
            emit("toggle-todo", index)
        }

        const deleteTodo = () => {
            emit("delete-todo", todoDeleteId.value)
            showModal.value = false;
            todoDeleteId.value = null;
        }

        return {
            showModal,
            openModal,
            closeModal,
            toggleTodo,
            deleteTodo,
            toastMessage,
            toastAlertType,
            showToast,
            triggerToast
        }
    }
}
</script>

<style>
    .card-body {
        display: block;
        margin-bottom: 10px;
        text-align: left;
    }

    .card-body button {
        margin-left: 10px;
    }
</style>