<script setup>
    const props = defineProps(['type', 'name', 'label', 'vmodel'])
</script>

<template>
    <label :for="props.name">
        {{ props.label }}
    </label>
    <input :type="props.type" :name="props.name" :v-model="props.vmodel" :id="props.name">
</template>

