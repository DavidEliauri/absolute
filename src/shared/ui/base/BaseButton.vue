<script setup lang="ts"></script>

<template>
    <button
        type="button"
        class="button"
    >
        <slot />
    </button>
</template>

<style lang="scss" scoped>
    .button {
        background-color: var(--accent);
        color: white;
        font-size: 14px;
        font-weight: 500;
        height: 38px;
        border-radius: 9999px;
        width: 100%;
        border: none;
    }
</style>
