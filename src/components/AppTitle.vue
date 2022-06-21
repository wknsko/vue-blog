<template>
    <h3>Pick Your Title</h3>
    <textarea type="text" v-model="title"></textarea>
    <app-button @action="addTitle" :title="!used ? 'Add Title' : 'Change Title'"></app-button>
    <app-button @action="clearTitle" v-if="used" title="Clear Title"></app-button>
</template>

<script>
import AppButton from './AppButton.vue';

export default {
    emits: ['passTitle', 'clearTitle'],
    inject: ['used'],
    data() {
        return {
            title: ''
        }
    },
    methods: {
        addTitle() {
            this.$emit('passTitle', this.title);
            this.used = true;
            this.$forceUpdate()

        },
        clearTitle() {
          this.title = '';
          this.$emit('clearTitle', this.title);
          this.used = false;
          this.$forceUpdate()



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
    height: 50px;
    margin-top: 20px;
    margin-bottom: 30px;
    outline: none;
    border: 2px solid #ccc;
    display: block;
    width: 97%;
    color: #2c3e50;
    padding-left: 5px;
    border-radius: 7px;
    font-size: 0.8rem;
}
button {
  margin-bottom: 10px;
}
</style>
