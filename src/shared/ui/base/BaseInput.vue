<script setup lang="ts">
    import { computed, useTemplateRef } from 'vue';

    export interface Props {
        modelValue: string;
        name?: string;
        type?: string;
        label?: string;
        beforeText?: string;
        error?: string;
    }

    const props = withDefaults(defineProps<Props>(), {
        type: 'text',
    });

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

    const inputNode = useTemplateRef('inputNode');

    defineExpose({
        inputNode,
    });
</script>

<template>
    <div
        :class="{
            'input-block_active': value,
            'input-block_with-before-text': !!props.beforeText,
        }"
        class="input-block"
    >
        <div class="input-block__wrapper">
            <div
                v-if="props.beforeText"
                class="input-block__before-text"
            >
                {{ props.beforeText }}
            </div>

            <input
                ref="inputNode"
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

        <div
            v-if="props.error"
            class="input-block__error"
        >
            {{ props.error }}
        </div>
    </div>
</template>

<style lang="scss" scoped>
    .input-block {
        &__wrapper {
            position: relative;
        }

        &_with-before-text {
            .input-block__input {
                padding-top: 0;
                text-align: right;
            }
        }

        &__error {
            color: lightcoral;
            line-height: 15px;
            font-size: 14px;
            font-weight: 300;
            margin-top: 4px;
        }

        &__before-text {
            position: absolute;
            left: 20px;
            top: calc(50% - 7.5px);
            line-height: 15px;
            font-size: 14px;
            font-weight: 300;
            color: var(--accent);
            pointer-events: none;
        }

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
            outline: none;

            &:focus {
                border-color: var(--accent);
            }
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
