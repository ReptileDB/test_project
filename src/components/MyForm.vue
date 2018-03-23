<template>
    <div>
        <div class="my-form-close-button d-none d-lg-block" @click="hideModal"><i class="fas fa-times"></i></div>
        <div class="my-header d-none d-lg-block">
            <h3>Ask a Question</h3>
            <div>Chat privately with a verified practitioner</div>
        </div>
        <div class="my-header-mobile d-lg-none">
            <div class="my-header-mobile-inner">
                <b-container >
                    <b-row class="pt-2 pb-2">
                        <b-col cols="10">
                            <h3>Ask a Question</h3>
                            <div>Chat privately with a verified practitioner</div>
                        </b-col>
                        <b-col cols="2"><div class="my-header-mobile-close-button" @click="hideModal"><i class="fas fa-times"></i></div></b-col>
                    </b-row>
                </b-container>
            </div>
        </div>
        <div class="my-form-body">
            <div class="my-form-body-inner">
                <b-form @submit="onSubmit" v-if="show" :novalidate="true">
                    <MyDropdown @change="onDropdownChange" :show-error="showErrorInDropdown" />
                    <div class="my-textarea-block">
                        <div>
                            <b-form-group id="inputGroup1">
                                <label for="my-textarea1" class="col-form-label pt-0">What's your question?</label>
                                <label for="my-textarea1" class="col-form-label pt-0 my-textarea-counter">{{getTextLength}}/{{limit}}</label>
                                <div :class="['my-textarea-block-inner', getTextareaStatus(), getTextareaFocus()] ">
                                    <b-form-textarea id="my-textarea1"
                                                     v-model="form.text"
                                                     placeholder="Ask a question or attach a file"
                                                     :rows="10"
                                                     :max-rows="10"
                                                     :no-resize=true
                                                     @focus.native="myTextareaFocus()"
                                                     @blur.native="myTextareaBlur()">
                                    </b-form-textarea>
                                    <b-container class="my-textarea-block-inner-bottom">
                                        <b-row class="pt-2 pb-2">
                                            <b-col cols="8">
                                                <MySelectFile @file-chosen="updateFile" :value="form.file"/>
                                            </b-col>
                                            <b-col cols="4"><b-button :class="['send-button', isValidForm? 'valid':'']" type="submit" variant="secondary">Send</b-button></b-col>
                                        </b-row>
                                    </b-container>
                                </div>
                            </b-form-group>
                        </div>
                    </div>
                </b-form>
                <div class="my-note">Note: Not for emergency use</div>
            </div>
        </div>
    </div>
</template>

<script>
    import MyDropdown from './MyDropdown.vue'
    import MySelectFile from './MySelectFile.vue'

    export default {
        name: 'MyForm',
        computed: {
            getTextLength: function () {
                return this.form.text.length;
            },
            isValidForm: function () {
                return this.isValidTextArea && this.form.typeOfQuestion;
            },
            isValidTextArea: function () {
                return this.form.file || this.getTextLength && (this.getTextLength <= this.limit);
            }
        },
        data () {
            return {
                showErrorInDropdown: false,
                showErrorInTextarea: false,
                limit: 500,
                textAreaFocus: false,
                form: {
                    text: '',
                    file: null,
                    typeOfQuestion: null,
                },
                show: true
            }
        },
        methods: {
            hideModal: function () {
                this.$root.$emit('bv::hide::modal','modal1')
            },
            onDropdownChange: function (result) {
                this.form.typeOfQuestion = result.selected;
                this.showErrorInDropdown = false;
            },
            myTextareaFocus: function () {
                this.textAreaFocus = true;
                this.showErrorInTextarea = false;
            },
            myTextareaBlur: function () {
                this.textAreaFocus = false;
            },
            getTextareaFocus: function () {
                return this.textAreaFocus ? ' is-focus': '';
            },
            getTextareaStatus: function () {
                return this.showErrorInTextarea? 'invalid': this.getTextLength? (this.isValidTextArea? 'valid': 'invalid'): '';
            },
            updateFile: function (file) {
                this.form.file = file;
              this.showErrorInTextarea = false;
            },
            formHasErrors: function () {
                let hasErrors = false;
                if (!this.form.typeOfQuestion) {
                    hasErrors = true;
                    this.showErrorInDropdown = true;
                }
                if (!this.isValidTextArea) {
                    hasErrors = true;
                    this.showErrorInTextarea = true;
                }
                return hasErrors;
            },
            onSubmit (evt) {
                evt.preventDefault();
                if (!this.formHasErrors()) {
                    alert(JSON.stringify(this.form));
                }
            }

        },
        components: {
            MyDropdown,
            MySelectFile
        }
    }
</script>

<style scoped>
    .my-form-close-button {
        position: absolute;
        width: 40px;
        height: 40px;
        background: #fff;
        border-radius: 40px;
        padding: 5px 12px;
        font-size: 21px;
        cursor: pointer;
        left: 120%;
        top: -20px;
    }
    .my-header {
        border-bottom: 0;
        text-align: center;
        padding-bottom: 2rem;
    }
    .my-header h3 {
        font-weight: bolder;
    }
    .my-header-mobile {
        border-bottom: 2px solid #e5e5e5;
        position: sticky;
        top: 0;
        background-color: #fff;
    }
    .my-header-mobile h3 {
        padding-bottom: 0;
        margin-bottom: 0;
    }
    .my-note {
        text-align: center;
        font-size: 11px;
        margin-top: 20px;
        padding-bottom: 20px;
    }
    .my-form-body {
        background-color: #fbfbfb;
    }
    .my-form-body-inner {
        max-width: 600px;
        width: auto;
        margin: auto;
        min-width: 300px;
        padding: 0 10px;
    }
    .my-textarea-counter {
        float: right;
    }
    #my-textarea1, #my-textarea1:focus {
        border: 0!important;
        box-shadow: none;
    }
    .my-textarea-block-inner {
        border: 1px solid #ced4da;
        border-radius: 0.25rem;
    }
    .my-textarea-block-inner.is-focus {
        border-color: #80bdff;
        box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
    }
    .my-textarea-block-inner.valid {
        border: 1px solid #80bdff;
    }
    .my-textarea-block-inner.valid.is-focus {
        border-color: #28a745;
        box-shadow: 0 0 0 0.2rem rgba(40, 167, 69, 0.25);
    }
    .my-textarea-block-inner.invalid {
        border-color: #dc3545;
    }
    .my-textarea-block-inner.invalid.is-focus {
        border-color: #dc3545;
        box-shadow: 0 0 0 0.2rem rgba(220, 53, 69, 0.25);
    }
    .my-textarea-block-inner-bottom {
        border-top: 1px solid #e7e7e7;
    }
    .send-button {
        border-radius: 1.25rem;
        padding-left: 2rem;
        padding-right: 2rem;
        float: right;
    }
    .send-button:focus {
        box-shadow: none;
    }
    .send-button.valid {
        background-color: #26ccc0;
        border-color: #26ccc0;
    }
    @media (min-width: 992px) {
        .my-form-body {
            background-color: #fff;
        }
        .my-form-body-inner {
            width: 600px;
        }
    }
    @media (max-width: 992px) {
        .my-header-mobile-inner {
            max-width: 600px;
            margin: auto;
            padding: 0 10px;
        }
        .my-header-mobile-inner .container {
            padding-left: 0;
            padding-right: 0;
            max-width: 1000px;
        }
        .my-header-mobile-close-button {
            position: absolute;
            right: -65px;
            margin-top: 10px;
            font-size: 20px;
            width: 40px;
            height: 40px;
            padding-top: 6px;
            padding-left: 10px;
        }
        .my-dropdown-block {
            padding-top: 20px;
        }
    }
    @media (max-width: 768px) {
        .my-header-mobile-inner{
            padding-left: 10px;
            padding-left: 10px;
        }
        .my-header-mobile-close-button {
            position: initial;
            margin-top: 10px;
            font-size: 20px;
            width: 40px;
            height: 40px;
            padding-top: 6px;
            padding-left: 10px;
        }
    }
</style>
