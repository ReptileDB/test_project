<template>
    <div class="my-select-file-block mt-1">
        <div v-if="value">
            <span @click="clearFiles" class="selected-file">{{value.name}} <i class="fas fa-times-circle"></i></span>
        </div>
        <div v-else @click="launchFilePicker" class="selected-file-block-inner">
            <i class="fas fa-paperclip"></i>&nbsp;
            <span :class="isShowError ? 'd-none d-sm-inline' : ''">Attach File</span>
        </div>
        <span v-if="isShowError" class="my-select-file-error selected-file" @click="clearFiles">File exceeds {{sizeLimit/1048576}}Mb limit  <i class="fas fa-times-circle"></i></span>
        <input type="file" ref="file" @change="handleFileChange" class="hidden-file-input" />
    </div>
</template>

<script>
    export default {
        name: 'MySelectFile',
        props: {
            value: File
        },
        data () {
            return {
                file: '',
                sizeLimit: 10485760,
                isShowError: false
            }
        },
        methods: {
            launchFilePicker: function () {
                this.$refs.file.click();
            },
            handleFileChange: function (e) {
                this.file = e.target.files[0];
                if (this.file && this.file.size && (this.file.size >= this.sizeLimit)){
                    this.isShowError = true;
                    this.file = null;
                }
                this.$emit('file-chosen', this.file);
            },
            clearFiles: function () {
                const input = this.$refs.file;
                input.type = 'text';
                input.type = 'file';
                this.file  = '';
                this.isShowError = false;
                this.$emit('file-chosen', null);
            }
        }
    }
</script>

<style scoped>
    .hidden-file-input {
        display: none;
    }
    .fa-paperclip {
        color: #b0b3b8;
    }
    .fa-times-circle {
        color: #5a6277;
        padding-top: 0.09rem;
    }
    .my-dropdown-block {
        cursor: pointer;
    }
    .selected-file {
        width: auto;
        background-color: #f1faf7;
        border-radius: 1.25rem;
        padding: 0.5rem;
    }
    .selected-file-block-inner {
        display: inline;
    }
    .my-select-file-error {
        font-size: 70%;
        color: red;
    }
</style>