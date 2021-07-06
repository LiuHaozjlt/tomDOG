<template>
<div>
  <h1>Register Form</h1>
  <form class="form" @submit.prevent="handleSubmit()">
    <BaseInput
      class="name"
      label="Name"
      v-model="form.name.$value"
      :errors="form.name.$errors"
      :validating="form.name.$validating"
      @blur="form.name.$onBlur()"
      placeholder="Alice, Bob, Oscar"
    />
    <BaseInput
      class="e-mail"
      label="Email address"
      v-model="form.email.$value"
      :errors="form.email.$errors"
      @blur="form.email.$onBlur()"
    />
    <BaseInput
      class="password"
      label="Password"
      type="password"
      v-model="form.password.$value"
      :errors="form.password.$errors"
      @blur="form.password.$onBlur()"
    />
    <BaseInput
      class="confirm-password"
      label="Confirm Password"
      type="password"
      v-model="form.confirmPassword.$value"
      :errors="form.confirmPassword.$errors"
      @blur="form.confirmPassword.$onBlur()"
    />
    <BaseButton
      class="signup"
      type="primary"
      htmlType="submit"
      :disabled="submitting"
    >
      Register
    </BaseButton>
    <BaseButton class="reset" @click="resetFields()">Reset</BaseButton>
  </form>
  <PreFormData :form="form" :errors="errors" />
  </div>
</template>

<script>
import BaseInput from "../components-demo/BaseInput.vue";
import BaseButton from "../components-demo/BaseButton.vue";
import PreFormData from "../components-demo/PreFormData.vue";
export default {
  components: { BaseInput, BaseButton, PreFormData },
  setup() {
    const password = ref("");
    const confirmPassword = ref("");
    const {
      form,
      errors,
      submitting,
      validateFields,
      resetFields
    } = useValidation({
      name: {
        $value: "",
        $rules: [
          required("Name is required"),
          min(3)("Name has to be longer than 2 characters"),
          name =>
            new Promise((resolve, reject) => {
              setTimeout(() => {
                if (["alice", "bob", "oscar"].includes(name.toLowerCase())) {
                  resolve();
                } else {
                  // Resolve or reject with a string
                  resolve("This name is already taken");
                }
              }, 2000);
            })
        ]
      },
      email: {
        $value: "",
        $rules: [email("Please enter a valid email address")]
      },
      password: {
        $value: password,
        $rules: [
          min(7)("Password has to be longer than 7 characters"),
          {
            key: "pw",
            rule: () =>
              password.value === confirmPassword.value ||
              "Passwords do not match"
          }
        ]
      },
      confirmPassword: {
        $value: confirmPassword,
        $rules: [
          min(7)("Password has to be longer than 7 characters"),
          {
            key: "pw",
            rule: () =>
              password.value === confirmPassword.value ||
              "Passwords do not match"
          }
        ]
      }
    });
    const handleSubmit = async () => {
      try {
        const formDate = await validateFields();
        alert(JSON.stringify(formDate, null, 2));
      } catch (e) {
        if (e instanceof ValidationError) {
          console.log(e.message);
        }
      }
    };
    return {
      form,
      errors,
      submitting,
      handleSubmit,
      resetFields
    };
  }
};
</script>