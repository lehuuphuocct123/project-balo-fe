<template>
  <div>
    <div
      class="flex items-center underline underline-offset-1 cursor-pointer"
      @click="back"
    >
      <step-backward-outlined />Trở lại
    </div>
    <div class="text-center font-bold text-[18px]">Thêm Người Dùng</div>
    <a-form
      :model="formState"
      autocomplete="off"
      @finish="onFinish"
      layout="vertical"
    >
      <a-form-item
        label="Họ Và Tên"
        name="name"
        :rules="[{ required: true, message: 'Trường này là bắt buộc!' }]"
      >
        <a-input v-model:value="formState.name" size="large" />
      </a-form-item>
      <a-form-item
        label="Email"
        name="email"
        :rules="[
          { required: true, message: 'Trường này là bắt buộc!' },
          { type: 'email', message: 'Trường này phải là Email' },
        ]"
      >
        <a-input v-model:value="formState.email" size="large" />
      </a-form-item>
      <a-form-item
        label="Tuổi"
        name="age"
        :rules="[{ required: true, message: 'Trường này là bắt buộc!' }]"
      >
        <a-input v-model:value="formState.age" size="large" />
      </a-form-item>

      <a-form-item
        label="Mật Khẩu"
        name="password"
        :rules="[{ required: true, message: 'Trường này là bắt buộc!' }]"
      >
        <a-input-password v-model:value="formState.password" size="large" />
      </a-form-item>

      <a-form-item
        label="Quyền"
        name="role"
        :rules="[{ required: true, message: 'Trường này là bắt buộc!' }]"
      >
        <a-radio-group v-model:value="formState.role">
          <a-radio :style="radioStyle" value="USER">
            <a-tag color="blue">USER</a-tag>
          </a-radio>
          <a-radio :style="radioStyle" value="ADMIN">
            <a-tag color="red">ADMIN</a-tag>
          </a-radio>
        </a-radio-group>
      </a-form-item>

      <div class="text-center">
        <a-button html-type="submit">Thêm Người Dùng</a-button>
      </div>
    </a-form>
  </div>
</template>
<script setup>
const notification = inject('notification')
const router = useRouter();
const formState = reactive({
  name: "",
  email: "",
  age: "",
  password: "",
  role: "USER",
});
const onFinish = async (values) => {
  try {
    await $fetch("http://localhost:5000/user", {
      method: "POST",
      body: values,
    });
    router.push('/user')
  } catch (error) {
    notification()
  }
};

const back = () => {
  router.push("/user");
};
</script>
