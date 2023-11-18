<template>
  <a-table :columns="columns" :data-source="data">
    <template #headerCell="{ column }">
      <template v-if="column.key === 'name'">
        <span> Họ Và Tên </span>
      </template>
    </template>

    <template #bodyCell="{ column, record }">
      <template v-if="column.key === 'name'">
        <div class="font-semibold">
          {{ record.name }}
        </div>
      </template>
      <template v-if="column.key === 'email'">
        <div class="text-[red]">
          {{ record.email }}
        </div>
      </template>
      <template v-if="column.key === 'role'">
        <span>
          <a-tag :color="record.role === 'ADMIN' ? 'red' : 'blue'">
            {{ record.role }}
          </a-tag>
        </span>
      </template>
      <template v-if="column.key === 'date'">
        <span>
          {{ $dayjs(record.createdAt).format('DD/MM/YYYY') }}
        </span>
      </template>
      <template v-else-if="column.key === 'action'">
        <div class="flex space-x-2">
          <a-button class="flex items-center" @click="updateUser(record.id)"
            ><highlight-outlined class="mr-[2px]" />Chỉnh Sửa</a-button
          >
          <a-button danger class="flex items-center" @click="deleteUser(record.id)"
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
  refresh: Function
})
const router = useRouter();
const columns = [
  {
    name: "Họ Và Tên",
    dataIndex: "name",
    key: "name",
  },
  {
    title: "Tuổi",
    dataIndex: "age",
    key: "age",
  },
  {
    title: "Email",
    dataIndex: "email",
    key: "email",
  },
  {
    title: "Quyền",
    key: "role",
    dataIndex: "role",
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
const deleteUser = async (id_user)=>{
  try {
    await $fetch(`http://localhost:5000/user/${id_user}`, {
      method: "DELETE"
    });
    props.refresh()
  } catch (error) {
    console.log(error);
  }
}
const updateUser = (id_user) => {
  router.push(`/user/${id_user}`);
};
</script>
