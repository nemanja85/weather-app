<script setup>
defineEmits(['close-modal']);
defineProps({
  modalActive: {
    type: Boolean,
    default: false,
  },
});
</script>

<template>
  <div
    v-show="modalActive"
    class="absolute top-0 left-0 flex justify-center w-full h-screen px-8 bg-slate-800 bg-opacity-30"
  >
    <div v-if="modalActive" class="self-start max-w-screen-md p-4 mt-32 bg-gray-100">
      <slot />
      <button @click="$emit('close-modal')" class="px-6 py-2 mt-8 text-gray-100 bg-blue-800">Close</button>
    </div>
  </div>
</template>
