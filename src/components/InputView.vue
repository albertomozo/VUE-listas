<script setup>
    import {  ref, defineEmits } from "vue";
    //const props = defineProps(["onNewItem"]);
    const text = ref("");
    const emits = defineEmits(["onNewItem"]);
    const MAX = 15;
    let quedan = ref(MAX);

    function handleKeyDown(evt) {
        if (evt.key === "Enter") {
            emits("onNewItem", text);
            text.value = "";
        }
        quedan = MAX - text.value.length;
    }
     
</script>

<template>
    <input v-model="text" @keydown="handleKeyDown" />{{ text }}
    <input  size="4" v-model="quedan" />
</template>

<style scoped></style>