<script setup lang="ts">
    import { BaseModal, MaskedInput, BaseSelect } from '@/shared/ui/';
    import { ref } from 'vue';

    const showModal = ref(false);

    const inputVar = ref('231');
    const selectVar = ref('');
</script>

<template>
    <div>
        <button @click="showModal = !showModal">Заполнить заявку</button>

        <Teleport to="body">
            <BaseModal
                @close="showModal = false"
                :show="showModal"
            >
                <template #title> Заполните заявку, чтобы стать резидентом </template>

                <template #default>
                    <MaskedInput
                        v-model="inputVar"
                        :mask="{
                            mask: '+{7} (000) 000-00-00',
                        }"
                        :input-props="{
                            label: 'Наименование организации / ИП',
                        }"
                    />

                    <BaseSelect
                        v-model="selectVar"
                        :options="[
                            {
                                id: '1',
                                text: 'Опция 1',
                            },
                            {
                                id: '2',
                                text: 'Опция 2',
                            },
                            {
                                id: '3',
                                text: 'Опция 3',
                            },
                        ]"
                        :multiple="false"
                        :label="'Наименование организации / ИП'"
                    />
                </template>
            </BaseModal>
        </Teleport>
    </div>
</template>
