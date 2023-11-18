<template>
  <div
    class="flex w-[100wh] h-[100vh] justify-center items-center background-login"
  >
    <div class="h-[400px] w-[370px] p-5 rounded-[10px] bg-white/[0.8]">
      <div class="text-center font-bold text-[25px] mb-5">ĐĂNG NHẬP BALO</div>
      <a-form
        :model="formState"
        autocomplete="off"
        layout="vertical"
        colon
        @finish="onFinish"
      >
        <a-form-item
          label="Email"
          name="email"
          :rules="[
            { required: true, message: 'Trường này không được bỏ trống!' },
            { type: 'email', message: 'Trường này phải là email!' },
          ]"
        >
          <a-input v-model:value="formState.email" size="large" />
        </a-form-item>

        <a-form-item
          label="Password"
          name="password"
          :rules="[
            { required: true, message: 'Trường này không được bỏ trống!' },
          ]"
        >
          <a-input-password v-model:value="formState.password" size="large" />
        </a-form-item>

        <a-button
          html-type="submit"
          class="w-full mt-[30px] text-white font-semibold"
          size="large"
          style="
            background: linear-gradient(
              83deg,
              rgba(247, 207, 0, 0.5802696078431373) 0%,
              rgba(255, 12, 0, 1) 100%
            );
          "
          >Đăng Nhập</a-button
        >
      </a-form>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  layout: "login",
});
const notification = inject('notification')
const router = useRouter()
const formState = ref({
  email: "",
  password: "",
});
const onFinish = async (values) => {
  try {
    const data = await $fetch("http://localhost:5000/login", {
      method: "POST",
      body: values,
    });
    useCookie('user_name').value = data.data.name
    router.push("/user");
  } catch (error) {
    notification()
  }
};
</script>

<style scoped>
.background-login {
  background-image: url("https://nonbaohiemdep.vn/wp-content/uploads/2022/11/balo-du-lich-2.jpg");
  background-repeat: no-repeat;
  background-size: 100%;
}
</style>
