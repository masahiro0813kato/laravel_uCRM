<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Head, Link } from "@inertiajs/vue3";
import FlashMessage from "@/Components/FlashMessage.vue";
import Pagination from "@/Components/Pagination.vue";
import { ref } from "vue";
import { Inertia } from "@inertiajs/inertia";

defineProps({
    customers: Object,
});

const search = ref("");
// ref の値を取得するには .valueが必要
const searchCustomers = () => {
    Inertia.get(route("customers.index", { search: search.value }));
};
</script>

<template>
    <Head title="顧客一覧" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="text-xl font-semibold leading-tight text-gray-800">顧客一覧</h2>
        </template>

        <div class="py-12">
            <div class="mx-auto max-w-7xl sm:px-6 lg:px-8">
                <div class="overflow-hidden bg-white shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">
                        <section class="text-gray-600 body-font">
                            <div class="container px-5 py-8 mx-auto">
                                <FlashMessage />
                                <div class="w-full mx-auto overflow-auto lg:w-2/3">
                                    <div class="flex w-full pl-4 mx-auto my-4 lg:w-full">
                                        <div>
                                            <input type="text" name="search" v-model="search" />
                                            <button class="px-2 py-2 text-white bg-blue-300" @click="searchCustomers">検索</button>
                                        </div>
                                        <Link as="button" :href="route('customers.create')" class="flex px-6 py-2 ml-auto text-white bg-indigo-500 border-0 rounded focus:outline-none hover:bg-indigo-600"> 顧客登録 </Link>
                                    </div>
                                    <table class="w-full text-left whitespace-no-wrap table-auto">
                                        <thead>
                                            <tr>
                                                <th class="px-4 py-3 text-sm font-medium tracking-wider text-gray-900 bg-gray-100 rounded-tl rounded-bl title-font">ID</th>
                                                <th class="px-4 py-3 text-sm font-medium tracking-wider text-gray-900 bg-gray-100 title-font">氏名</th>
                                                <th class="px-4 py-3 text-sm font-medium tracking-wider text-gray-900 bg-gray-100 title-font">カナ</th>
                                                <th class="px-4 py-3 text-sm font-medium tracking-wider text-gray-900 bg-gray-100 title-font">電話番号</th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <tr v-for="customer in customers.data" :key="customer.id">
                                                <td class="px-4 py-3 border-b-2 border-gray-200">
                                                    {{ customer.id }}
                                                </td>
                                                <td class="px-4 py-3 border-b-2 border-gray-200">
                                                    {{ customer.name }}
                                                </td>
                                                <td class="px-4 py-3 border-b-2 border-gray-200">
                                                    {{ customer.kana }}
                                                </td>
                                                <td class="px-4 py-3 border-b-2 border-gray-200">
                                                    {{ customer.tel }}
                                                </td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                            <Pagination class="mt-6" :links="customers.links"></Pagination>
                        </section>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
