<template>
  <a-table :columns="columns" :data-source="data" :scroll="{ x: 1300 }">
    <template #headerCell="{ column }">
      <template v-if="column.key === 'product_name'">
        <span> Tên Sản Phẩm </span>
      </template>
    </template>

    <template #bodyCell="{ column, record }">
      <template v-if="column.key === 'product_name'">
        <div class="font-semibold">
          {{ record.product_name }}
        </div>
      </template>
      <template v-if="column.key === 'image'">
        <img
          class="w-[100px] h-[100px]"
          :src="`http://localhost:5000/upload?image=${record.image}`"
        />
      </template>
      <template v-if="column.key === 'price'">
        <div class="font-semibold">
          {{ new Intl.NumberFormat().format(record.price) }} đ
        </div>
      </template>
      <template v-if="column.key === 'date'">
        <span>
          {{ $dayjs(record.createdAt).format("DD/MM/YYYY") }}
        </span>
      </template>
      <template v-else-if="column.key === 'action'">
        <div class="flex space-x-2">
          <a-button
            class="flex items-center"
            @click="updateUser(record.product_id)"
            ><highlight-outlined class="mr-[2px]" />Chỉnh Sửa</a-button
          >
          <a-button
            danger
            class="flex items-center"
            @click="deleteUser(record.id)"
            ><delete-outlined class="mr-[2px]" />Xóa</a-button
          >
        </div>
      </template>
    </template>
  </a-table>
</template>
<script setup>
const props = defineProps({
  data: Array,
});
const router = useRouter();
const columns = [
  {
    name: "Tên Sản Phẩm",
    dataIndex: "product_name",
    key: "product_name",
  },
  {
    title: "Ảnh",
    dataIndex: "image",
    key: "image",
  },
  {
    title: "Thể Loại",
    dataIndex: "category_name",
    key: "category_name",
  },
  {
    title: "Màu",
    dataIndex: "color",
    key: "color",
  },
  {
    title: "Giá",
    key: "price",
    dataIndex: "price",
  },
  {
    title: "Mô Tả",
    key: "description",
    dataIndex: "description",
    with: "40%",
  },
  {
    title: "Ngày Tạo",
    key: "date",
    dataIndex: "date",
  },
  {
    title: "Thao Tác",
    key: "action",
  },
];
const deleteUser = async (id) => {
  try {
    await $fetch(`http://localhost:5000/user/${id}`, {
      method: "DELETE",
    });
    props.refresh();
  } catch (error) {
    console.log(error);
  }
};
const updateUser = (id) => {
  router.push(`/product/${id}`);
};
</script>