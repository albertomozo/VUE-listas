<script setup>
    const props = defineProps(['msg']);   
</script>

<template>
    <button>{{ msg }}</button>
</template>

<style scoped>

button {
    background-color: rgb(17, 8, 108);
    color: rgb(255, 255, 255)
}

button:hover {
    background-color: rgb(108, 8, 68);
    color: rgb(255, 255, 255);
    font-size : 20px;
}
</style>
