<script setup lang="ts">
    import { computed, ref } from 'vue';

    interface Option {
        id: string;
        text: string;
    }

    type Props = {
        modelValue: string | string[];
        options: Option[];
        multiple?: boolean;
        label?: string;
    };

    const props = defineProps<Props>();

    const emit = defineEmits<{
        'update:modelValue': [value: string | string[]];
    }>();

    const value = computed({
        get() {
            return props.modelValue;
        },
        set(newValue) {
            emit('update:modelValue', newValue);
        },
    });

    function isMultiple(a: string | string[]): a is string[] {
        return Array.isArray(a);
    }

    const selectedOptions = computed(() => {
        return props.options.filter((option) => {
            if (isMultiple(value.value)) {
                return value.value.includes(option.id);
            } else {
                return option.id === value.value;
            }
        });
    });

    function onOptionClick(id: string) {
        if (isMultiple(value.value)) {
            if (value.value.includes(id)) {
                value.value = value.value.filter((item) => item !== id);

                return;
            }

            value.value = [...value.value, id];

            return;
        }

        if (value.value === id) {
            value.value = '';

            return;
        }

        value.value = id;
    }

    const showOptions = ref(false);
</script>

<template>
    <div
        :class="{ 'select-block_active': selectedOptions.length }"
        class="select-block"
    >
        <input
            :value="value"
            type="hidden"
        />

        <label
            v-if="props.label"
            class="select-block__label"
        >
            {{ props.label }}
        </label>

        <button
            @click="showOptions = !showOptions"
            class="select-block__select"
        >
            <span>
                {{ selectedOptions.map((option) => option.text).join(', ') }}
            </span>

            <svg class="select-block__icon">
                <use
                    v-bind="{
                        'xlink:href': '/sprite.svg#chevrone-down',
                    }"
                ></use>
            </svg>
        </button>

        <div
            v-if="showOptions"
            class="select-block__options"
        >
            <ul class="options">
                <li
                    v-for="option in props.options"
                    :key="option.id"
                    class="options__item"
                >
                    <button
                        @click="onOptionClick(option.id)"
                        class="options__button"
                    >
                        {{ option.text }}
                    </button>
                </li>
            </ul>
        </div>
    </div>
</template>

<style lang="scss" scoped>
    .select-block {
        position: relative;

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

        &__select {
            text-align: left;
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
            background-color: white;

            &:focus {
                border-color: var(--accent);
            }
        }

        &_active &__label {
            top: 10px;
        }

        &__options {
            position: absolute;
            top: 100%;
            left: 0;
            margin-top: 8px;
            width: 100%;
        }

        &__icon {
            display: block;
            width: 20px;
            height: 20px;
            position: absolute;
            top: 22px;
            right: 20px;
            pointer-events: none;
        }
    }

    .options {
        background-color: white;
        box-shadow:
            0 1px 3px 0 rgb(0 0 0 / 0.1),
            0 1px 2px -1px rgb(0 0 0 / 0.1);
        border-radius: 4px;
        padding: 8px;

        &__item {
            margin-top: 4px;

            &:first-child {
                margin-top: 0;
            }
        }

        &__button {
            background: none;
            border: none;
            padding: 0;
            width: 100%;
            text-align: left;
            font-family: 'Montserrat', sans-serif;
            font-weight: 500;
            font-size: 16px;
            color: var(--accent);
            cursor: pointer;
        }
    }
</style>
