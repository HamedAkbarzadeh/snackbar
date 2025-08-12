<template>
    <transition :name="transitionName">
        <div
            v-show="visibale"
            v-bind="$attrs"
            class="fixed left-1/2 transform -translate-x-1/2 px-4 py-3 rounded-lg shadow-md flex justify-between items-center space-x-4 z-50 min-w-80 max-w-[90%]"
            :class="[
                snackBg[type],
                position === 'top' ? 'top-4' : 'bottom-4'
            ]"
        >
            <!-- icon -->
            <div class="pr-1 text-white">
                <slot name="icon">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                        stroke-width="1.5" stroke="currentColor" class="size-6">
                        <path stroke-linecap="round" stroke-linejoin="round" :d="snackIcon[type]" />
                    </svg>
                </slot>
            </div>

            <!-- message -->
            <div class="pr-6 " :class="snackText[type]">
                <slot name="message" />
            </div>

            <!-- undo -->
            <span v-show="undo" class="hover:text-slate-100 text-sm font-medium cursor-pointer"
                :class="snackDoneColor[type]" @click="handleUndo">
                {{ $attrs.dir === 'rtl' ? 'برگشت' : 'undo' }}
            </span>

            <!-- done -->
            <span v-show="done" class="hover:text-slate-100 text-sm font-medium cursor-pointer"
                :class="snackDoneColor[type]" @click="handleClose">
                {{ $attrs.dir === 'rtl' ? 'بستن' : 'done' }}
            </span>
        </div>
    </transition>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { snackBg, snackDoneColor, snackIcon, snackText } from '../constants/snackbar.constants';

type AlertType = keyof typeof snackBg;

const props = defineProps<{
    type: AlertType,
    done?: boolean,
    undo?: boolean,
    duration?: number,
    position?: 'top' | 'bottom'
}>();

const emits = defineEmits(['close', 'undo', 'open']);

const visibale = ref(false);

const transitionName = computed(() => {
    return props.position === 'top' ? 'slide-down' : 'slide-up';
});

const autoVisible = () => {
    setTimeout(() => {
        handleClose();
    }, props.duration ?? 3000);
};

const handleOpen = () => {
    autoVisible();
    visibale.value = true;
    setTimeout(() => {
        emits('open');
    }, 300);
};

const handleClose = () => {
    visibale.value = false;
    setTimeout(() => {
        emits('close');
    }, 300);
};

const handleUndo = () => {
    visibale.value = false;
    setTimeout(() => {
        emits('undo');
    }, 300);
};

defineExpose({
    handleOpen,
    handleClose
});
</script>

<style scoped>
/* پایین به بالا */
.slide-up-enter-active,
.slide-up-leave-active {
    transition: all 0.3s ease;
}
.slide-up-enter-from {
    transform: translate(-50%, 100%);
    opacity: 0;
}
.slide-up-enter-to {
    transform: translate(-50%, 0);
    opacity: 1;
}
.slide-up-leave-from {
    transform: translate(-50%, 0);
    opacity: 1;
}
.slide-up-leave-to {
    transform: translate(-50%, 100%);
    opacity: 0;
}

/* بالا به پایین */
.slide-down-enter-active,
.slide-down-leave-active {
    transition: all 0.3s ease;
}
.slide-down-enter-from {
    transform: translate(-50%, -100%);
    opacity: 0;
}
.slide-down-enter-to {
    transform: translate(-50%, 0);
    opacity: 1;
}
.slide-down-leave-from {
    transform: translate(-50%, 0);
    opacity: 1;
}
.slide-down-leave-to {
    transform: translate(-50%, -100%);
    opacity: 0;
}
</style>
