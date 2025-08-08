<template>
    <transition name="slide-up">
        <div v-show="visibale"
            class="fixed bottom-4 left-1/2 transform -translate-x-1/2 px-4 py-3 rounded-lg shadow-md flex justify-between items-center space-x-4 z-50 min-w-[300px] max-w-[90%]"
            :class="snackBg[alertType]">
            <div class="pr-6 " :class="snackText[alertType]">
                {{ message }}
            </div>
            <span v-show="undo" class=" hover:text-slate-100 text-sm font-medium cursor-pointer "
                :class="snackDoneColor[alertType]" @click="handleUndo">
                undo
            </span>
            <span v-show="done" class=" hover:text-slate-100 text-sm font-medium cursor-pointer "
                :class="snackDoneColor[alertType]" @click="handleClose">
                done
            </span>
        </div>
    </transition>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { snackBg, snackDoneColor, snackText } from '../constants/snackbar.constants';

type AlertType = keyof typeof snackBg;

const props = defineProps<{
    alertType: AlertType,
    message: string,
    done?: boolean,
    undo?: boolean,
    duration?: number
}>()


const emits = defineEmits(['close', 'undo', 'open'])


const visibale = ref(false);

const autoVisible = () => {
    setTimeout(() => {
        handleClose();
    }, props.duration ?? 3000);
};

const handleOpen = () => {
    autoVisible();
    console.log('open snack');

    visibale.value = true;
    setTimeout(() => {
        emits('open');
    }, 300);
}


const handleClose = () => {
    visibale.value = false;
    setTimeout(() => {
        emits('close');
    }, 300);
}

const handleUndo = () => {
    visibale.value = false;
    setTimeout(() => {
        emits('undo');
    }, 300);
}



defineExpose({
    handleOpen,
    handleClose
});
</script>

<style scoped>
.slide-up-enter-active,
.slide-up-leave-active {
    transition: all 0.3s ease;
}

.slide-up-enter-from,
.slide-up-leave-to {
    transform: translateY(100%);
    opacity: 0;
}
</style>
