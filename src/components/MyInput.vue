<script setup lang="ts">
import { ref } from 'vue'
const emit = defineEmits(['response']);

let mychat = ref("");
emit('response', mychat)
</script>

<template>
    <input type="text" v-model="mychat" />
</template>