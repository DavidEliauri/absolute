<script setup lang="ts">
    import { BaseModal } from '@/shared/ui/';
    import { ref } from 'vue';
    import { FormRequestAdd } from '@/widgets/form-request-add/';

    const showModal = ref(false);
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
                    <FormRequestAdd />
                </template>
            </BaseModal>
        </Teleport>
    </div>
</template>
