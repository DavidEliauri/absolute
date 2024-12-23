<script setup lang="ts">
    import { reactive, ref } from 'vue';
    import { BaseInput, MaskedInput, BaseSelect, BaseDatepicker, BaseButton } from '@/shared/ui';

    interface FormField {
        value: string;
        error: string;
    }

    interface FormDateField {
        value: Date | null;
        error: string;
    }

    const form: {
        name: FormField;
        phone: FormField;
        room: FormField;
        address: FormField;
        squareFrom: FormField;
        squareTo: FormField;
        dateFrom: FormDateField;
        dateTo: FormDateField;
    } = reactive({
        name: {
            value: '',
            error: '',
        },
        phone: {
            value: '',
            error: '',
        },
        room: {
            value: '',
            error: '',
        },
        address: {
            value: '',
            error: '',
        },
        squareFrom: {
            value: '',
            error: '',
        },
        squareTo: {
            value: '',
            error: '',
        },
        dateFrom: {
            value: null,
            error: '',
        },
        dateTo: {
            value: null,
            error: '',
        },
    });

    const submitStatus = ref<'idle' | 'pending' | 'success' | 'error'>('idle');

    function onSubmit() {
        form.name.error = '';
        form.phone.error = '';
        form.room.error = '';
        form.address.error = '';
        form.squareFrom.error = '';
        form.squareTo.error = '';
        form.dateFrom.error = '';
        form.dateTo.error = '';

        let hasError = false;

        if (form.name.value === '') {
            form.name.error = 'Поле обязательно для заполнения';

            hasError = true;
        }

        if (form.phone.value === '') {
            form.phone.error = 'Поле обязательно для заполнения';

            hasError = true;
        }

        if (form.room.value === '') {
            form.room.error = 'Поле обязательно для заполнения';

            hasError = true;
        }

        if (form.address.value === '') {
            form.address.error = 'Поле обязательно для заполнения';

            hasError = true;
        }

        if (form.squareFrom.value === '') {
            form.squareFrom.error = 'Поле обязательно для заполнения';

            hasError = true;
        }

        if (form.squareTo.value === '') {
            form.squareTo.error = 'Поле обязательно для заполнения';

            hasError = true;
        }

        if (form.dateFrom.value === null) {
            form.dateFrom.error = 'Поле обязательно для заполнения';

            hasError = true;
        }

        if (form.dateTo.value === null) {
            form.dateTo.error = 'Поле обязательно для заполнения';

            hasError = true;
        }

        if (hasError) {
            submitStatus.value = 'error';

            return;
        }

        submitStatus.value = 'pending';

        // Async request

        submitStatus.value = 'success';
    }
</script>

<template>
    <form
        @submit.prevent="onSubmit"
        class="form"
    >
        <div class="form__grid">
            <BaseInput
                v-model="form.name.value"
                :error="form.name.error"
                name="orgName"
                label="Наименование организации / ИП"
            />

            <MaskedInput
                v-model="form.phone.value"
                :mask="{
                    mask: '+{7} (000) 000-00-00',
                }"
                :input-props="{
                    error: form.phone.error,
                    label: 'Контактный телефон',
                }"
            />

            <BaseSelect
                v-model="form.room.value"
                :options="[
                    {
                        id: '1',
                        text: 'Производственная площадь',
                    },
                    {
                        id: '2',
                        text: 'Другая площадь',
                    },
                ]"
                :error="form.room.error"
                label="Тип помещения"
            />

            <BaseInput
                v-model="form.address.value"
                :error="form.address.error"
                name="address"
                label="Адрес"
            />

            <div class="form__line">
                <div class="form__line-title">Площадь помещения (м<sup>2</sup>)</div>

                <div class="form__line-elements">
                    <BaseInput
                        v-model="form.squareFrom.value"
                        :error="form.squareFrom.error"
                        name="squareFrom"
                        beforeText="от"
                    />

                    <BaseInput
                        v-model="form.squareTo.value"
                        :error="form.squareTo.error"
                        name="squareTo"
                        beforeText="до"
                    />
                </div>
            </div>

            <div class="form__line">
                <div class="form__line-title">Дата начала аренды</div>

                <div class="form__line-elements">
                    <BaseDatepicker
                        v-model="form.dateFrom.value"
                        :error="form.dateFrom.error"
                        name="dateFrom"
                        beforeText="c"
                    />

                    <BaseDatepicker
                        v-model="form.dateTo.value"
                        :minDate="form.dateFrom.value ?? undefined"
                        :error="form.dateTo.error"
                        name="dateTo"
                        beforeText="по"
                    />
                </div>
            </div>
        </div>

        <div class="form__button">
            <BaseButton @click="onSubmit"> Отправить </BaseButton>
        </div>

        <div
            v-if="submitStatus === 'success'"
            :class="{ form__message_success: submitStatus === 'success' }"
            class="form__message"
        >
            Заявка успешно отправлена
        </div>
    </form>
</template>

<style lang="scss" scoped>
    .form {
        &__grid {
            & > * {
                margin-top: 20px;

                &:first-child {
                    margin-top: 0;
                }
            }
        }

        &__line {
        }

        &__line-elements {
            display: flex;
            flex-direction: column;
            gap: 8px;

            & > * {
                @media (min-width: 768px) {
                    flex: 0 0 50%;
                }
            }

            @media (min-width: 768px) {
                flex-direction: row;
            }
        }

        &__line-title {
            vertical-align: super;
            font-size: smaller;
            line-height: 15px;
            font-size: 14px;
            font-weight: 300;
            color: var(--accent);
            margin-bottom: 20px;
        }

        &__button {
            margin-top: 40px;

            @media (min-width: 768px) {
                max-width: 160px;
                margin-left: auto;
                margin-right: auto;
            }
        }

        &__message {
            margin-top: 20px;
            padding: 10px 20px;
            border-radius: 4px;
            color: #0f5132;
            background-color: #d1e7dd;
            border-color: #badbcc;
            border: 1px solid;
        }
    }
</style>
