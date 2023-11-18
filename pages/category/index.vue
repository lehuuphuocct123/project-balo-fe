<template>
  <div>
    <div class="text-center font-bold text-[18px]">Hãng Balo</div>
    <a-button class="my-5" @click="openCreate = true"> Thêm Hãng </a-button>
    <CategoryTable :data="dataCategory" :getDataCategory="getDataCategory" :refresh="refresh" />
    <a-modal v-model:visible="openCreate" title="Tạo Hãng" :footer="false">
      <a-form :model="formState" @finish="createCategry" layout="vertical">
        <a-form-item
          label="Tên Hãng"
          name="name"
          :rules="[{ required: true, message: 'Trường này bắt buộc!' }]"
        >
          <a-input v-model:value="formState.name" />
        </a-form-item>

        <div class="flex justify-end space-x-2 items-center">
          <a-button html-type="submit">Tạo Hãng</a-button>
          <a-button danger @click="turnOffModal">Hủy</a-button>
        </div>
      </a-form>
    </a-modal>
    <a-modal v-model:visible="openUpdate" title="Chỉnh Sửa" :footer="false">
      <a-form :model="formState" @finish="updateCategory" layout="vertical">
        <a-form-item
          label="Tên Hãng"
          name="name"
          :rules="[{ required: true, message: 'Trường này bắt buộc!' }]"
        >
          <a-input v-model:value="formState.name" />
        </a-form-item>

        <div class="flex justify-end space-x-2 items-center">
          <a-button html-type="submit">Lưu lại</a-button>
          <a-button danger @click="turnOffModal">Hủy</a-button>
        </div>
      </a-form>
    </a-modal>
  </div>
</template>

<script setup>
const notification = inject('notification')
const openCreate = ref(false);
const openUpdate = ref(false);
const formState = ref({
  name: "",
});
const id_update = ref(undefined)
const { data, pending, refresh } = await useFetch(
  "http://localhost:5000/category"
);

const turnOffModal = () => {
  formState.value.name = "";
  openCreate.value = false;
  openUpdate.value = false;
};

const dataCategory = computed(() => {
  if (!pending.value) {
    return data.value.data;
  }
  return [];
});

const getDataCategory = (data)=>{
    id_update.value = data.id
    formState.value = {
        name: data.name
    }
    openUpdate.value = true;
}

const createCategry = async (values) => {
  try {
    await $fetch("http://localhost:5000/category", {
      method: "POST",
      body: values,
    });
    refresh()
    turnOffModal();
  } catch (error) {
    notification()
  }
};

const updateCategory = async (values) => {
  try {
    await $fetch(`http://localhost:5000/category/${id_update.value}`, {
      method: "PUT",
      body: values,
    });
    refresh()
    turnOffModal();
  } catch (error) {
    notification()
  }
};
</script>
