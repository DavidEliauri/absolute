<script setup lang="ts">
    defineProps<{
        show?: boolean;
    }>();

    defineEmits<{
        close: [];
    }>();
</script>

<template>
    <div
        v-if="show"
        @click.self="$emit('close')"
        class="modal"
    >
        <div class="modal__wrapper">
            <div class="modal__title">
                <slot name="title" />
            </div>

            <div class="modal__content">
                <slot />
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
    .modal {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        background: rgba($color: #585858a3, $alpha: 0.64);
        padding: 0 16px;

        &__wrapper {
            width: 100%;
            background-color: white;
            max-width: 800px;
            padding: 20px;
            max-height: 100%;
            overflow: auto;

            @media (min-width: 768px) {
                padding: 40px;
            }
        }

        &__title {
            font-weight: 600;
            font-size: 18px;
            margin-bottom: 20px;

            @media (min-width: 768px) {
                font-size: 24px;
                margin-bottom: 40px;
            }
        }
    }
</style>
