<template>
    <div class="my-dropdown-block">
        <b-form-group id="inputGroup1"
                      label="What can we help you with?"
                      label-for="exampleInput1">
            <b-form-select id="exampleInput1"
                           :options="options"
                           required
                           class="my-dropdown"
                           v-model="selected"
                           :state= "showError? 'invalid': 'null'">
            </b-form-select>
            <div class="my-dropdown-error">
                <span v-show="showError">This field is required</span>&nbsp;
            </div>
        </b-form-group>
    </div>
</template>

<script>
    export default {
        name: 'MyDropdown',
        props: ['showError'],
        data () {
            return {
                selected: null,
                options: [
                    { value: null, text: 'Please select', disabled: true },
                    { value: 1, text: 'Infant Feeding' },
                    { value: 2, text: 'Sleeping' },
                    { value: 3, text: 'Pain &Illness' },
                    { value: 4, text: 'Diet/Nutrition and Wellness' },
                    { value: 5, text: 'Mental Health' },
                    { value: 6, text: 'Birth Recovery' },
                    { value: 7, text: 'General Question About Baby' },
                    { value: 8, text: 'Customer Service' },
                ]
            }
        },
        watch: {
            'selected': function() {
                this.$emit('change', { selected: this.selected });
            }
        },
    }
</script>

<style scoped>
    .my-dropdown {
        cursor: pointer;
        background-image: url('../assets/dropdown-arrow.png')!important;
    }
    .my-dropdown-error {
        margin-top: 0.25rem;
        font-size: 80%;
        color: #dc3545;
    }
</style>