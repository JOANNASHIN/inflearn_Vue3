<template>
<div class="modal-wrapper">
    <div class="modal-dialog">
        <div class="modal-content">
        <header class="modal-header">
                <h5 class="modal-title" id="staticBackdropLabel">
                    <slot name="title"></slot>
                </h5>
                <button 
                    type="button"
                    class="btn-close" 
                    data-bs-dismiss="modal"
                    @click="onClose"
                ></button>
        </header>

        <div class="modal-body">
            <slot name="body"></slot>
        </div>

        <div class="modal-footer">
            <slot name="footer"></slot>
        </div>
    </div>
  </div>
</div>
</template>

<script>

export default {    
    name: "ModalComponent",
    emits: ["close"],
    setup(props, { emit }) {
        const onClose = () => {
            emit("close")
        }

        return {
            onClose,
        }
    }
}
</script>

<style>
    .modal-wrapper {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.5);
    }
</style>