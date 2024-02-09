<script setup>
    const props = defineProps(['type', 'text'])
</script>

<template>
    <button :type="props.type">
        {{ props.text }}
    </button>
</template>