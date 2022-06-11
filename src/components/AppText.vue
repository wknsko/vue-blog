<template>
    <h3>Add a Piece of Text</h3>
    <textarea type="text" v-model="text"></textarea>
    <app-button @action="addText" title="Add Text"></app-button>
</template>

<script>
import AppButton from './AppButton.vue'

export default {
    emits: ['passText'],
    data() {
        return {
            text: ''
        }
    },
    methods: {
        addText() {
            this.$emit('passText', this.text);
            this.text = ''
        }
    },
    components: { AppButton },

}
</script>

<style>
textarea {
    margin: 0 auto;
    outline: none;
    border: 2px solid #ccc;
    display: block;
    width: 97%;
    height: 250px;
    color: #2c3e50;
    padding-left: 5px;
    border-radius: 7px;
    font-size: 0.8rem;
    margin-bottom: 20px;
}
</style>