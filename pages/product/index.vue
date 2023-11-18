<template>
  <div>
    <div class="text-center font-bold text-[18px]">QUẢN LÝ SẢN PHẨM</div>
    <div class="flex justify-between mb-5">
      <a-button @click="addProduct"> Thêm Sản Phẩm </a-button>
      <div class="flex space-x-2">
        <a-input v-model:value="search" />
        <a-button @click="refresh">Tìm Kiếm</a-button>
      </div>
    </div>
    <ProductTable :data="dataProduct" />
  </div>
</template>
<script setup>
const router = useRouter();
const search = ref("");
const { data, pending, refresh } = await useFetch(
  "http://localhost:5000/product",
  {
    params: { search },
    watch: false,
  }
);
const addProduct = () => {
  router.push("/product/add-product");
};

const dataProduct = computed(() => {
  if (!pending.value) {
    return data.value.data;
  }
  return [];
});
</script>
