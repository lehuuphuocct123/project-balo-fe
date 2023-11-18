<template>
  <a-table :columns="columns" :data-source="data">
    <template #headerCell="{ column }">
      <template v-if="column.key === 'name'">
        <span>Tên Hãng</span>
      </template>
    </template>

    <template #bodyCell="{ column, record }">
      <template v-if="column.key === 'name'">
        <div class="font-semibold">
          {{ record.name }}
        </div>
      </template>
      <template v-if="column.key === 'date'">
        <span>{{ $dayjs(record.createdAt).format('DD/MM/YYYY') }}</span>
      </template>
      <template v-else-if="column.key === 'action'">
        <div class="flex space-x-2">
          <a-button class="flex items-center" @click="getDataCategory(record)"
            ><highlight-outlined class="mr-[2px]" />Chỉnh Sửa</a-button
          >
          <a-button danger class="flex items-center" @click="deleteCategory(record.id)"
            ><delete-outlined class="mr-[2px]" />Xóa</a-button
          >
        </div>
      </template>
    </template>
  </a-table>
</template>
<script setup>
const props = defineProps({
  getDataCategory: Function,
  refresh: Function,
  data: Array
})
const columns = [
  {
    name: "Tên",
    dataIndex: "name",
    key: "name",
    width: '50%',
    align: 'center',
  },
  {
    title: "Ngày tạo",
    key: "date",
    dataIndex: "date",
  },
  {
    title: "Thao Tác",
    key: "action",
  },
];

const deleteCategory = async (id)=>{
  try {
    await $fetch(`http://localhost:5000/category/${id}`, {
      method: "DELETE"
    });
    props.refresh()
  } catch (error) {
    console.log(error);
  }
}
</script>
