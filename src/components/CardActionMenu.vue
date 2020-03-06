<template>
    <div class="card-action-menu">
        <button class="delete"><i class="fas fa-trash"></i></button>
    </div>
</template>

<script>
export default {
    name: 'CardActionMenu'
}
</script>

<style scoped lang="scss">
    .card-action-menu {
        position: absolute;
        top: 2rem;
        right: -3rem;
        z-index: 0;
        background: #d30000;
        border-radius: 0 2rem 2rem 0;
        box-shadow: 1px 0 3px rgba(0, 0, 0, 0.5);
        padding: 0.5rem 1rem;
        display: flex;
        flex-direction: column;
        justify-content: space-between;

        button {
            border: none;
            font-size: 1.5rem;
            background: none;
            color: #ffffff;
            cursor: pointer;
        }
    }
</style>