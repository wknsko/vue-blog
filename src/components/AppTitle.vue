<template>
    <h3>Pick Your Title</h3>
    <textarea type="text" v-model="title"></textarea>
    <app-button @action="addTitle" :title="used ? 'Add Title' : 'Change Title'"></app-button>
</template>

<script>
import AppButton from './AppButton.vue';

export default {
    emits: ['passTitle'],
    data() {
        return {
            title: '',
            used: true,
        }
    },
    methods: {
        addTitle() {
            this.$emit('passTitle', this.title)
            this.used = false
        }
    },
    components: {
        AppButton
    }

}

//https://images.pexels.com/photos/10890741/pexels-photo-10890741.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1
</script>

<style scoped>
textarea {
    height: 50px
}
</style>