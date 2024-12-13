<script setup lang="ts">
    import { BaseModal, MaskedInput } from '@/shared/ui/';
    import { ref } from 'vue';

    const showModal = ref(false);

    const inputVar = ref('231');
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
                </template>
            </BaseModal>
        </Teleport>
    </div>
</template>
