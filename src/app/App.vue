<script setup lang="ts">
    import { BaseModal, BaseInput } from '@/shared/ui/';
    import { ref } from 'vue';

    const showModal = ref(false);

    const inputVar = ref('');
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
                    <BaseInput
                        v-model="inputVar"
                        label="Наименование организации / ИП"
                    />
                </template>
            </BaseModal>
        </Teleport>
    </div>
</template>
