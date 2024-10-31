<script setup>
import Chirp from "@/Components/Chirp.vue";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Head, useForm } from "@inertiajs/vue3";

const form = useForm({
    message: "",
});

defineProps(['chirps'])
</script>

<template>

    <Head title="Chirps" />
    <AuthenticatedLayout>
        <div class="max-w-2xl mx-auto p-4 sm:p-6 lg:p-8">
            <form @submit.prevent="form.post(route('chirps.store'), { onSuccess: () => form.reset() })"
                class="grid gap-4">
                <Textarea v-model="form.message" />
                <InputError :message="form.errors.message" class="mt-2" />
                <div>
                    <Button type="submit" label="Chirp" />
                </div>
            </form>

            <div class="mt-6 bg-white shadow-sm rounded-lg divide-y">
                <Chirp v-for="chirp in chirps" :key="chirp.id" :chirp="chirp" />
            </div>
        </div>
    </AuthenticatedLayout>
</template>
