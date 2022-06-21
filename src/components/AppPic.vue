<template>
    <h3>Add a URL of a Picture</h3>
    <input type="text" v-model="url">
    <app-button @action="addPicture" title="Add Picture"></app-button>
</template>

<script>
import AppButton from './AppButton.vue'

export default {
    emits: ['passPicture'],
    data() {
        return {
            url: ''
        }
    },
    methods: {
        addPicture() {
            this.$emit('passPicture', this.url);
            this.url = ''
        }
    },
    components: {
        AppButton
    }

}
</script>

<style scoped>
input {
    height: 30px;
    width: 100%;
    margin-top: 30px;

}
</style>
