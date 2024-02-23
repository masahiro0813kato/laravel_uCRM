<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Head } from "@inertiajs/vue3";
import { reactive } from "vue";
import { Inertia } from "@inertiajs/inertia";
import BreeezeValidationErrors from "@/Components/ValidationErrors.vue";
import { Core as YubinBangoCore } from "yubinbango-core2";

defineProps({
    errors: Object,
});

const form = reactive({
    name: null,
    kana: null,
    tel: null,
    email: null,
    postcode: null,
    address: null,
    birthday: null,
    gender: null,
    memo: null,
});

const storeCustomer = () => {
    Inertia.post("/customers", form);
};

// 数字を文字に変換 第1引数が郵便番号、第2がコールバックで引数に住所
const fetchAddress = () => {
    new YubinBangoCore(String(form.postcode), (value) => {
        form.address = value.region + value.locality + value.street;
    });
};
</script>

<template>
    <Head title="顧客登録" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="text-xl font-semibold leading-tight text-gray-800">顧客登録</h2>
        </template>

        <div class="py-12">
            <div class="mx-auto max-w-7xl sm:px-6 lg:px-8">
                <div class="overflow-hidden bg-white shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">
                        <section class="relative text-gray-600 body-font">
                            <BreeezeValidationErrors :errors="errors" />
                            <form @submit.prevent="storeCustomer">
                                <div class="container px-5 py-24 mx-auto">
                                    <div class="mx-auto lg:w-1/2 md:w-2/3">
                                        <div class="flex flex-wrap -m-2">
                                            <div class="w-full p-2">
                                                <div class="relative">
                                                    <label for="name" class="text-sm leading-7 text-gray-600">顧客名</label>
                                                    <input v-model="form.name" type="text" id="name" name="name" class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200" />
                                                </div>
                                            </div>
                                            <div class="w-full p-2">
                                                <div class="relative">
                                                    <label for="kana" class="text-sm leading-7 text-gray-600">顧客名カナ</label>
                                                    <input v-model="form.kana" type="text" id="kana" name="kana" class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200" />
                                                </div>
                                            </div>
                                            <div class="w-full p-2">
                                                <div class="relative">
                                                    <label for="tel" class="text-sm leading-7 text-gray-600">電話番号</label>
                                                    <input v-model="form.tel" type="number" id="tel" name="tel" class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200" />
                                                </div>
                                            </div>
                                            <div class="w-full p-2">
                                                <div class="relative">
                                                    <label for="email" class="text-sm leading-7 text-gray-600">メールアドレス</label>
                                                    <input v-model="form.email" type="email" id="email" name="email" class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200" />
                                                </div>
                                            </div>
                                            <div class="w-full p-2">
                                                <div class="relative">
                                                    <label for="postcode" class="text-sm leading-7 text-gray-600">郵便番号</label>
                                                    <input v-model="form.postcode" @change="fetchAddress" type="number" id="postcode" name="postcode" class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200" />
                                                </div>
                                            </div>
                                            <div class="w-full p-2">
                                                <div class="relative">
                                                    <label for="address" class="text-sm leading-7 text-gray-600">住所</label>
                                                    <input v-model="form.address" type="text" id="address" name="address" class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200" />
                                                </div>
                                            </div>
                                            <div class="w-full p-2">
                                                <div class="relative">
                                                    <label for="birthday" class="text-sm leading-7 text-gray-600">誕生日</label>
                                                    <input v-model="form.birthday" type="date" id="birthday" name="birthday" class="w-full px-3 py-1 text-base leading-8 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200" />
                                                </div>
                                            </div>
                                            <div class="w-full p-2">
                                                <div class="relative">
                                                    <label class="text-sm leading-7 text-gray-600">性別</label><br />

                                                    <input v-model="form.gender" value="0" type="radio" id="gender0" name="gender" />
                                                    <label for="gender0" class="ml-2 mr-4">男</label>

                                                    <input v-model="form.gender" value="1" type="radio" id="gender1" name="gender" />
                                                    <label for="gender1" class="ml-2 mr-4">女</label>

                                                    <input v-model="form.gender" value="2" type="radio" id="gender2" name="gender" />
                                                    <label for="gender2" class="ml-2 mr-4">その他</label>
                                                </div>
                                            </div>
                                            <div class="w-full p-2">
                                                <div class="relative">
                                                    <label for="memo" class="text-sm leading-7 text-gray-600">メモ</label>
                                                    <textarea id="memo" name="memo" v-model="form.memo" class="w-full h-32 px-3 py-1 text-base leading-6 text-gray-700 transition-colors duration-200 ease-in-out bg-gray-100 bg-opacity-50 border border-gray-300 rounded outline-none resize-none focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-200"></textarea>
                                                </div>
                                            </div>

                                            <div class="w-full p-2">
                                                <button class="flex px-8 py-2 mx-auto text-lg text-white bg-indigo-500 border-0 rounded focus:outline-none hover:bg-indigo-600">登録する</button>
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
