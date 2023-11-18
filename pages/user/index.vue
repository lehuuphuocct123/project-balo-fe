<template>
  <div>
    <div class="text-center font-bold text-[18px]">QUẢN LÝ NGƯỜI DÙNG</div>
    <div class="flex space-x-10 py-10">
      <a-card class="flex-1 card-all">
        <a-statistic :value="dataUser.length">
          <template #title>
            <span class="text-[#333] font-medium">Tất Cả</span>
          </template>
        </a-statistic>
      </a-card>
      <a-card class="flex-1 card-user">
        <a-statistic :value="dataUser.filter((e) => e.role === 'USER').length">
          <template #title>
            <span class="text-[#333] font-medium">Người Dùng</span>
          </template>
        </a-statistic>
      </a-card>
      <a-card class="flex-1 card-admin">
        <a-statistic :value="dataUser.filter((e) => e.role === 'ADMIN').length">
          <template #title>
            <span class="text-[#333] font-medium">Quản Trị Viên</span>
          </template></a-statistic
        >
      </a-card>
    </div>
    <a-button @click="addUser" class="mb-5"> Thêm Người Dùng </a-button>
    <UserTable v-if="!pending" :data="dataUser" :refresh="refresh" />
  </div>
</template>
<script setup>
const router = useRouter();
const { data, pending, refresh } = await useFetch("http://localhost:5000/user");
const addUser = () => {
  router.push("/user/add-user");
};

const dataUser = computed(() => {
  if (!pending.value) {
    return data.value.data;
  }
  return [];
});
</script>

<style>
.card-all {
  background: linear-gradient(
    90deg,
    rgba(3, 245, 2, 0.5802696078431373) 0%,
    rgba(135, 244, 176, 0.6166841736694677) 100%
  );
}
.card-user {
  background: linear-gradient(
    90deg,
    rgba(4, 0, 247, 0.5802696078431373) 0%,
    rgba(135, 244, 176, 0.6166841736694677) 100%
  );
}
.card-admin {
  background: linear-gradient(
    90deg,
    rgba(247, 0, 0, 0.5802696078431373) 0%,
    rgba(244, 182, 135, 0.6166841736694677) 100%
  );
}
</style>
