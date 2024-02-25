<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
//import { Head } from "@inertiajs/vue3";
import { Head, Link } from "@inertiajs/inertia-vue3";
import { onMounted, reactive, ref, computed } from "vue";
import { Inertia } from "@inertiajs/inertia";
import BreeezeValidationErrors from "@/Components/ValidationErrors.vue";
import dayjs from "dayjs";

const props = defineProps({
    errors: Object,
    items: Array,
    order: Array,
});

// ページ読み込み後 即座に実行
onMounted(() => {
    console.log(props.items);
    console.log(props.order[0]);
});
</script>

<template>
    <Head title="購買履歴 詳細画面" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="text-xl font-semibold leading-tight text-gray-800">購買履歴 詳細画面</h2>
        </template>

        <div class="py-12">
            <div class="mx-auto max-w-7xl sm:px-6 lg:px-8">
                <div class="overflow-hidden bg-white shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">
                        <section class="relative text-gray-600 body-font">
                            <BreeezeValidationErrors :errors="errors" />
                            <form @submit.prevent="storePurchase">
                                <div class="container px-5 py-24 mx-auto">
                                    <div class="mx-auto lg:w-1/2 md:w-2/3">
                                        <div class="flex flex-wrap -m-2">
                                            <div class="w-full p-2">
                                                <div class="relative">
                                                    <label for="date" class="text-sm leading-7 text-gray-600">日付</label>
                                                    <div name="date" class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200">
                                                        {{ dayjs(props.order[0].created_at).format("YYYY-MM-DD HH:mm:ss") }}
                                                    </div>
                                                </div>
                                            </div>

                                            <div class="w-full p-2">
                                                <div class="relative">
                                                    <label for="customer" class="text-sm leading-7 text-gray-600">会員名</label>
                                                    <div name="date" class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200">
                                                        {{ props.order[0].customer_name }}
                                                    </div>
                                                </div>
                                            </div>

                                            <div class="w-full mx-auto mt-8 overflow-auto">
                                                <table class="w-full text-left whitespace-no-wrap table-auto">
                                                    <thead>
                                                        <tr>
                                                            <th class="px-4 py-3 text-sm font-medium tracking-wider text-gray-900 bg-gray-100 rounded-tl rounded-bl title-font">ID</th>
                                                            <th class="px-4 py-3 text-sm font-medium tracking-wider text-gray-900 bg-gray-100 title-font">商品名</th>
                                                            <th class="px-4 py-3 text-sm font-medium tracking-wider text-gray-900 bg-gray-100 title-font">金額</th>
                                                            <th class="px-4 py-3 text-sm font-medium tracking-wider text-gray-900 bg-gray-100 title-font">数量</th>
                                                            <th class="px-4 py-3 text-sm font-medium tracking-wider text-gray-900 bg-gray-100 title-font">小計</th>
                                                        </tr>
                                                    </thead>
                                                    <tbody>
                                                        <tr v-for="item in props.items">
                                                            <td class="px-4 py-3 border-b-2 border-gray-200">
                                                                {{ item.item_id }}
                                                            </td>
                                                            <td class="px-4 py-3 border-b-2 border-gray-200">
                                                                {{ item.item_name }}
                                                            </td>
                                                            <td class="px-4 py-3 border-b-2 border-gray-200">
                                                                {{ item.item_price }}
                                                            </td>
                                                            <td class="px-4 py-3 border-b-2 border-gray-200">
                                                                {{ item.quantity }}
                                                            </td>
                                                            <td class="px-4 py-3 text-lg text-gray-900 border-b-2 border-gray-200">
                                                                {{ item.subtotal }}
                                                            </td>
                                                        </tr>
                                                    </tbody>
                                                </table>

                                                <div class="w-full p-2">
                                                    <div>
                                                        <label class="text-sm leading-7 text-gray-600">ステータス</label>
                                                        <div v-if="props.order[0].status == false" class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200">キャンセル済み</div>
                                                        <div v-if="props.order[0].status == true" class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200">未キャンセル</div>
                                                    </div>
                                                </div>

                                                <div class="w-full p-2">
                                                    <div>
                                                        <label class="text-sm leading-7 text-gray-600">キャンセル日</label>
                                                        <div v-if="props.order[0].status == false" class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200">
                                                            {{ dayjs(props.order[0].update_at).format("YYYY-MM-DD HH:mm:ss") }}
                                                        </div>
                                                    </div>
                                                </div>

                                                <div class="w-full p-2">
                                                    <div>
                                                        <label class="text-sm leading-7 text-gray-600">合計</label>
                                                        <div class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200">{{ props.order[0].total }} 円</div>
                                                    </div>
                                                </div>
                                            </div>
                                            <div v-if="props.order[0].status == true" class="w-full p-2">
                                                <Link
                                                    as="button"
                                                    :href="
                                                        route('purchases.edit', {
                                                            purchase: props.order[0].id,
                                                        })
                                                    "
                                                    class="flex px-8 py-2 mx-auto text-lg text-white bg-indigo-500 border-0 rounded focus:outline-none hover:bg-indigo-600"
                                                    >編集する</Link
                                                >
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </form>
                        </section>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
