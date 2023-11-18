<template>
  <div>
    <div
      class="flex items-center underline underline-offset-1 cursor-pointer"
      @click="back"
    >
      <step-backward-outlined />Trở lại
    </div>
    <div class="text-center font-bold text-[18px]">Chỉnh Sửa Sản Phẩm</div>
    <a-form
      :model="formState"
      autocomplete="off"
      @finish="onFinish"
      layout="vertical"
    >
      <a-form-item
        label="Tên Sản Phẩm"
        name="name"
        :rules="[{ required: true, message: 'Trường này là bắt buộc!' }]"
      >
        <a-input v-model:value="formState.name" size="large" />
      </a-form-item>
      <a-form-item
        label="Ảnh Sản Phẩm"
        name="image"
        :rules="[{ required: true, message: 'Trường này là bắt buộc!' }]">
        <input type="file" @change="uploadImage" accept="image/*"/>
      </a-form-item>
      <img v-if="formState.image" :src="`http://localhost:5000/upload?image=${formState.image}`" class="w-[300px] h-[400px]"/>
      <a-form-item
        label="Thể Loại"
        name="category_id"
        :rules="[{ required: true, message: 'Trường này là bắt buộc!' }]"
      >
        <a-select
          v-model:value="formState.category_id"
          placeholder="Thể Loại"
          size="large"
        >
          <a-select-option
            :value="category.id"
            v-for="category in categorys"
            :key="category.id"
            >{{ category.name }}</a-select-option
          >
        </a-select>
      </a-form-item>
      <a-form-item
        label="Màu Sắc"
        name="color"
        :rules="[{ required: true, message: 'Trường này là bắt buộc!' }]"
      >
        <a-input v-model:value="formState.color" size="large" />
      </a-form-item>

      <a-form-item
        label="Giá"
        name="price"
        :rules="[{ required: true, message: 'Trường này là bắt buộc!' }]"
      >
        <a-input v-model:value="formState.price" size="large" />
      </a-form-item>

      <a-form-item
        label="Số Lượng"
        name="quantity"
        :rules="[{ required: true, message: 'Trường này là bắt buộc!' }]"
      >
        <a-input v-model:value="formState.quantity" size="large" />
      </a-form-item>

      <a-form-item
        label="Mô Tả"
        name="description"
        :rules="[{ required: true, message: 'Trường này là bắt buộc!' }]"
      >
        <a-textarea
          v-model:value="formState.description"
          placeholder="Mô Tả"
          :rows="4"
        />
      </a-form-item>

      <div class="text-center">
        <a-button html-type="submit">Cập Nhật Sản Phẩm</a-button>
      </div>
    </a-form>
  </div>
</template>
<script setup>
const notification = inject('notification')
const router = useRouter();
const id = router.currentRoute.value.params.id;
const { data, pending } = await useFetch(`http://localhost:5000/product/${id}`);
const { data: dataCategory, pending: peningCategory } = await useFetch(
  "http://localhost:5000/category"
);
const formState = ref({
  name: "",
  image: "",
  category_id: "",
  color: "",
  quantity: "",
  price: "",
  description: "",
});
const categorys = computed(() => {
  if (!peningCategory.value) {
    return dataCategory.value.data;
  }
  return [];
});
const onFinish = async (values) => {
  try {
    await $fetch(`http://localhost:5000/product/${id}`, {
      method: "PUT",
      body: values,
    });
    router.push("/product");
  } catch (error) {
    notification()
  }
};

const back = () => {
  router.push("/product");
};

watchEffect(() => {
  if (!pending.value) {
    formState.value = data.value.data;
  }
});
</script>
