<template>
    <form @submit.prevent ="$emit('addTask')">
        <input
            type="text"
            placeholder="Type and hit enter..."
            :value="modelValue"
            class="composer__input"
            @keyup="edit"
            @keypress="$emit('update:modelValue',$event.target.value)"
        />
    </form>
</template>

<script>
export default {
    name: 'Composer',
    emits:['addTask','update:modelValue'],

    props:['modelValue'],

}
</script>

<style>
</style>