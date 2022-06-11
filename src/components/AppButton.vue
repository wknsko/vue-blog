<template>
    <button @click="$emit('action')" class="btn">{{ title }}</button>
</template>

<script>
export default {
    emits: ['action'],
    props: ['title'],
    methods: {

    }


}
</script>

<style>
</style>