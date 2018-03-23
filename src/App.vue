<template>
    <div id="app">
        <b-btn v-b-modal.modal1>Launch modal</b-btn>

        <b-modal id="modal1" title="Bootstrap-Vue" ref="myModalRef"
                 class="my-modal"
                 :hide-footer="true"
                 :hide-header="true"
                 size="lg"
                 centered>
            <MyForm/>
        </b-modal>
    </div>
</template>

<script>
import MyForm from './components/MyForm.vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

export default {
    name: 'app',
    components: {
        MyForm
    }
}
</script>

<style>
#app {
    font-family: Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #5a6277;
}
@media (max-width: 992px) {
    .my-modal .modal-dialog {
        max-width: 1000px;
        margin: 0;
    }
    .my-modal .modal-content{
        border: 0;
        border-radius: 0;
        background-color: #fbfbfb;
    }
    .my-modal .modal-body {
        padding: 0;
    }
    .my-modal .modal-dialog-centered {
        align-items: inherit;
    }
}
</style>
