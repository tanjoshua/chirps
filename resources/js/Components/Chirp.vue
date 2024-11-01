<script setup>
import { Link, useForm } from "@inertiajs/vue3";
import { ref } from "vue";
import Dropdown from "./Dropdown.vue";
import InputError from "./InputError.vue";

const props = defineProps(['chirp']);

const form = useForm({
    message: props.chirp.message,
});

const isEditing = ref(false);

const toggleEditing = () => {
    isEditing.value = !isEditing.value;
}

const onCancelEditing = () => {
    isEditing.value = false;
    form.reset();
    form.clearErrors();
}

</script>

<template>
    <div class="p-6 flex space-x-2">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-600 -scale-x-100" fill="none"
            viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round"
                d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z" />
        </svg>
        <div class="flex-1">
            <div class="flex justify-between items-center">
                <div>
                    <span class="text-gray-800">{{ chirp.user.name }}</span>
                    <small class="ml-2 text-sm text-gray-600">{{ new Date(chirp.created_at).toLocaleString() }}</small>
                    <small v-if="chirp.created_at !== chirp.updated_at" class="text-sm text-gray-600">
                        &middot;edited
                    </small>
                </div>
                <Dropdown v-if="chirp.user.id === $page.props.auth.user.id">
                    <template #trigger>
                        <Button icon="pi pi-ellipsis-v" text rounded />
                    </template>
                    <template #content>
                        <button
                            class="block w-full px-4 py-2 text-left text-sm leading-5 text-gray-700 hover:bg-gray-100 focus:bg-gray-100 transition duration-150 ease-in-out"
                            @click="toggleEditing">
                            Edit
                        </button>
                        <Link as="button"
                            class="block w-full px-4 py-2 text-left text-sm leading-5 text-gray-700 hover:bg-gray-100 focus:bg-gray-100 transition duration-150 ease-in-out"
                            method="delete" :href="route('chirps.destroy', chirp.id)">
                        Delete
                        </Link>
                    </template>
                </Dropdown>
            </div>

            <form v-if="isEditing"
                @submit.prevent="form.put(route('chirps.update', chirp.id), { onSuccess: () => isEditing = false })">
                <Textarea v-model="form.message" />
                <InputError :message="$page.props.errors.message" class="mt-2" />
                <div class="space-x-2">
                    <Button type="submit" label="Save" />
                    <Button label="Cancel" @click="onCancelEditing" severity="secondary" />
                </div>
            </form>
            <p v-else class="mt-4 text-lg text-gray-900">{{ chirp.message }}</p>
        </div>
    </div>
</template>