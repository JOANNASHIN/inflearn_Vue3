<template>
  <ModalComponent>
      <template v-slot:title>Delete Todo</template>
      <template v-slot:body>Are you sure you want to delete the todo?</template>
      <template v-slot:footer><button 
            type="button" 
            class="btn btn-secondary" 
            data-bs-dismiss="modal"
            @click="onClose"
        >
            cancel
        </button>
        <button 
            type="button" 
            class="btn btn-danger"
            @click="onDelete"
        >
            Delete</button>
      </template>

  </ModalComponent>

</template>

<script>
import ModalComponent from "@/components/ModalComponent.vue";
export default {
    components: {
        ModalComponent
    },
    emits: ["close", "delete"],
    setup(props, { emit }) {
        const onClose = () => {
            emit("close")
        }

        const onDelete = () => {
             emit("delete")
        }

        return {
            onClose,
            onDelete
        }
    }
}
</script>

<style>

</style>