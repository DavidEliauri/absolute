<script setup lang="ts">
    import { computed, onMounted, ref, useTemplateRef, watch } from 'vue';
    import IMask, { type InputMask, type FactoryOpts } from 'imask';
    import { BaseInput } from '@/shared/ui/';
    import { type Props as BaseInputProps } from '@/shared/ui/base/BaseInput.vue';

    interface Props {
        modelValue: string;
        mask: FactoryOpts;
        inputProps: Omit<BaseInputProps, 'modelValue'>;
    }

    const props = defineProps<Props>();

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

    type BaseInputType = InstanceType<typeof BaseInput>;

    const baseInputComponent = useTemplateRef<BaseInputType>('baseInput');

    let maskClass: InputMask;

    onMounted(() => {
        maskClass = IMask(baseInputComponent.value!.inputNode!, props.mask);

        value.value = maskClass.unmaskedValue;

        maskClass.on('accept', () => {
            value.value = maskClass.unmaskedValue;
        });
    });

    watch(value, () => {
        maskClass.value = value.value;

        value.value = maskClass.unmaskedValue;
    });

    const childModelValue = ref(value.value);
</script>

<template>
    <BaseInput
        ref="baseInput"
        v-model="childModelValue"
        v-bind="props.inputProps"
    />
</template>
