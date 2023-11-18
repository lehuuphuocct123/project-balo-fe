<template>
  <div>
    <slot />
    <div class="fixed top-5 left-0 right-0 flex justify-center" v-if="notify">
      <span
        class="py-[10px] px-5 bg-[#f76767] rounded-[20px] text-white font-semibold"
        >Đã có lỗi xảy ra, Vui lòng kiểm tra lại!</span
      >
    </div>
  </div>
</template>

<script setup>
const notify = ref(false);
const timeOutNoti = ref(null);

const notification = () => {
  clearTimeout(timeOutNoti.value);
  notify.value = true;
  timeOutNoti.value = setTimeout(() => {
    notify.value = false;
  }, 3000);
};

provide("notification", notification);
</script>
