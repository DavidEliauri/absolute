<script setup lang="ts">
    import { reactive } from 'vue';
    import { BaseInput, MaskedInput, BaseSelect, BaseDatepicker } from '@/shared/ui';

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
</script>

<template>
    <form class="form">
        <BaseInput
            v-model="form.name.value"
            name="orgName"
            label="Наименование организации / ИП"
        />

        <MaskedInput
            v-model="form.phone.value"
            :mask="{
                mask: '+{7} (000) 000-00-00',
            }"
            :input-props="{
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
            label="Тип помещения"
        />

        <BaseInput
            v-model="form.address.value"
            name="address"
            label="Адрес"
        />

        <div class="form__line">
            <div class="form__line-title">Площадь помещения (м<sup>2</sup>)</div>

            <div class="form__line-elements">
                <BaseInput
                    v-model="form.squareFrom.value"
                    name="squareFrom"
                    beforeText="от"
                />

                <BaseInput
                    v-model="form.squareTo.value"
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
                    name="dateFrom"
                    beforeText="c"
                />

                <BaseDatepicker
                    v-model="form.dateTo.value"
                    :minDate="form.dateFrom.value ?? undefined"
                    name="dateTo"
                    beforeText="по"
                />
            </div>
        </div>
    </form>
</template>

<style lang="scss" scoped>
    .form {
        & > * {
            margin-top: 20px;

            &:first-child {
                margin-top: 0;
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
    }
</style>
