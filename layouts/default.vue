<template>
  <div>
    <a-layout>
      <a-layout-header class="header flex justify-between background-header">
        <div class="logo" />
        <div class="flex items-center">
          <a-popover :title="false">
            <template #content>
              <div class="min-h-5 cursor-pointer" @click="logout">
                Đăng Xuất
              </div>
            </template>
            <div class="cursor-pointer flex items-center">
              <a-avatar>{{ useCookie("user_name").value?.[0] }}</a-avatar>
              <span class="ml-[4px] text-white font-semibold text-[16px]">{{
                useCookie("user_name").value
              }}</span>
            </div>
          </a-popover>
        </div>
      </a-layout-header>
      <a-layout class="h-[calc(100vh-64px)]">
        <a-layout-sider width="200" class="slide">
          <div class="font-bold py-5 text-center">DANH MỤC</div>
          <div
            v-for="item in items"
            :key="item.path"
            class="py-[10px] px-3 font-semibold cursor-pointer"
            :class="{
              'slide-active': router.currentRoute.value.path.includes(
                item.path
              ),
            }"
            @click="redirectPage(item.path)"
          >
            {{ item.name }}
          </div>
        </a-layout-sider>
        <a-layout style="padding: 0 24px 24px">
          <a-layout-content
            :style="{
              background: '#fff',
              padding: '24px',
              margin: 0,
              minHeight: '280px',
            }"
            class="max-h-[calc(100vh-64px)] overflow-y-auto"
          >
            <slot />
          </a-layout-content>
        </a-layout>
      </a-layout>
    </a-layout>
    <div class="fixed top-5 left-0 right-0 flex justify-center" v-if="notify">
      <span
        class="py-[10px] px-5 bg-[#f76767] rounded-[20px] text-white font-semibold"
        >Đã có lỗi xảy ra, Vui lòng kiểm tra lại!</span
      >
    </div>
  </div>
</template>
<script setup>
const router = useRouter();
console.log(router.currentRoute.value.path);
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
const items = [
  {
    name: "Người Dùng",
    path: "/user",
  },
  {
    name: "Hãng",
    path: "/category",
  },
  {
    name: "Sản Phẩm",
    path: "/product",
  },
];

const redirectPage = (path) => {
  router.push(path);
};

const logout = () => {
  router.push("/login");
};
</script>
<style scope>
.background-header {
  background: linear-gradient(
    83deg,
    rgba(150, 234, 198, 0.58) 0%,
    rgba(0, 38, 255, 0.617) 100%
  ) !important;
}
.slide {
  background: linear-gradient(
    183deg,
    rgba(150, 234, 198, 0.58) 0%,
    rgba(0, 38, 255, 0.617) 100%
  ) !important;
}

.slide-active {
  background: linear-gradient(
    200deg,
    rgba(169, 197, 226, 0.5802696078431373) 0%,
    rgba(0, 41, 165, 0.617) 100%
  );
  color: white;
  border-right: 4px solid blue;
}
</style>
