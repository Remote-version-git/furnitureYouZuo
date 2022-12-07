<template>
    <div class="furniture-steps">
        <slot></slot>
    </div>
</template>

<script>
export default {
    name: 'FurnitureSteps'
}
</script>

<style lang="scss" scoped>
.furniture-steps {
    display: flex;
    flex-direction: column;
}
</style>