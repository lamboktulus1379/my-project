<script setup lang="ts">
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

defineProps<{
    name: string
    message: string
    currentClass: string
}>()
</script>

<template>
    <div class="wrapper" :class="currentClass">
        <font-awesome-icon :color="currentClass == 'end' ? 'blue' : ''" icon="user" title="name" />
        <div class="message">{{ message }}</div>
    </div>
</template>

<style scoped>
.wrapper {
    display: flex;
}

.end {
    justify-content: end;
}

.message {
    margin-left: 4px;
}

.start {
    justify-content: start;
}
</style>