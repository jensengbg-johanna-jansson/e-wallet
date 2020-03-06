<template>
    <button class="primary__btn" :class="{ dark__btn: buttonStyleDark }">{{ buttonText }}</button>
</template>

<script>
export default {
    name: 'PrimButton',
    props: {
        buttonText: String,
        buttonStyleDark: Boolean
    }
}
</script>

<style scoped>
    .primary__btn {
        width: 100%;
        border: 1px solid #000000;
        border-radius: 0.5rem;
        padding: 1rem;
        font-size: 1.375rem;
        text-transform: uppercase;
        font-weight: bold;
        margin-top: 2rem;
    }
    .dark__btn {
        background: #000000;
        color: #ffffff;
    }
</style>