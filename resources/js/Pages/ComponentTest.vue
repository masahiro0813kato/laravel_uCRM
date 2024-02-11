<script setup>
import GuestLayout from "@/Layouts/GuestLayout.vue";
import Label from "@/Components/InputLabel.vue";
import Inpot from "@/Components/TextInput.vue";

const emitTest = (e) => console.log(e);
</script>

<template>
    <GuestLayout>
        <Label value="件名">タイトル</Label>
        <Inpot
            modelValue="初期値が入ります"
            @update:modelValue="emitTest"
        ></Inpot>
    </GuestLayout>
</template>
