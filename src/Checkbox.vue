<template>
    <label>
        <input type="checkbox" v-model="model">
        {{ label }}
    </label>
</template>

<script setup>
const model = defineModel()
//Parents to children
defineProps({
    label: String
});
//Children to parents
const emits = defineEmits(['check', 'uncheck']);
const onChange = (event) => {
    if (event.currentTarget.checked) {
        emits('check', event.currentTarget)
    } else {
        emits('uncheck', event.currentTarget)
    }
}
</script>