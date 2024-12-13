<script setup lang="ts">
    import { computed } from 'vue';

    const props = withDefaults(
        defineProps<{
            modelValue: string;
            name?: string;
            type?: string;
            label?: string;
        }>(),
        {
            type: 'text',
        },
    );

    const emit = defineEmits<{
        'update:modelValue': [value: string];
    }>();

    const value = computed({
        get() {
            return props.modelValue;
        },

        set(newValue) {
            emit('update:modelValue', newValue);
        },
    });
</script>

<template>
    <div
        :class="{
            'input-block_active': value,
        }"
        class="input-block"
    >
        <input
            v-model="value"
            :name="props.name"
            :type="props.type"
            class="input-block__input"
        />

        <label
            v-if="props.label"
            class="input-block__label"
        >
            {{ props.label }}
        </label>
    </div>
</template>

<style lang="scss" scoped>
    .input-block {
        position: relative;

        &__input {
            width: 100%;
            height: 64px;
            border: 2px solid var(--border);
            border-radius: 6px;
            color: var(--accent);
            font-family: 'Montserrat', sans-serif;
            font-weight: 500;
            font-size: 18px;
            padding: 28px 20px 0;
        }

        &__label {
            position: absolute;
            left: 0;
            top: calc(50% - 7.5px);
            width: 100%;
            padding: 0 20px;
            line-height: 15px;
            font-size: 14px;
            font-weight: 300;
            color: var(--accent);
            pointer-events: none;
            transition: top 0.2s ease;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        &__input:focus + &__label,
        &_active &__label {
            top: 10px;
        }
    }
</style>
