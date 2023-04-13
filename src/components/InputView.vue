<script setup>
    import {  ref, defineEmits,nextTick } from "vue";
    //const props = defineProps(["onNewItem"]);
    const text = ref("");
    const emits = defineEmits(["onNewItem"]);
    const MAX = 8;
    let quedan = ref(MAX);

    function handleKeyDown(evt) {
        if (evt.key === "Enter") {
            emits("onNewItem", text);
            text.value = "";
        }
        quedan = MAX - text.value.length;
        if (quedan <= 0)
        {
            alert('solo puedes escribir '  + MAX + ' caracteres ' + text.value);
            nextTick(() => {
                 text.value = text.value.slice(0,MAX);

            }); 
             
        }
    }
     
</script>

<template>
    <input v-model="text" @keydown="handleKeyDown" />{{ text }}
    <input  size="4" v-model="quedan" />
</template>

<style scoped></style>