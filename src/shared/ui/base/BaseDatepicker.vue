<script setup lang="ts">
    import { computed, onMounted, toRef, useTemplateRef, watch } from 'vue';
    import AirDatepicker from 'air-datepicker';

    interface Props {
        modelValue: Date | null;
        name?: string;
        beforeText?: string;
        minDate?: Date;
        error?: string;
    }

    const props = defineProps<Props>();

    const minDate = toRef(props, 'minDate');

    const emit = defineEmits<{
        'update:modelValue': [value: Date | null];
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

    let datepicker: AirDatepicker;

    onMounted(() => {
        datepicker = new AirDatepicker(inputNode.value!, {
            isMobile: true,
            autoClose: true,
            minDate: minDate.value ?? new Date(),
            onSelect: ({ date }) => {
                if (Array.isArray(date)) {
                    value.value = date[0];

                    return;
                }

                value.value = date;
            },
            selectedDates: value.value ? [value.value] : undefined,
        });
    });

    watch(minDate, () => {
        datepicker.update({
            minDate: minDate.value,
        });
    });
</script>

<template>
    <div
        :class="{
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
                :name="props.name"
                autocomplete="off"
                class="input-block__input"
            />
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
    }
</style>
