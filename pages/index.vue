<template>
  <main class="main">
    <v-form @submit.prevent="validate()" class="form">
      <v-container>
        <v-progress-linear
          :active="loading"
          :indeterminate="loading"
          color="success" striped
          height="5"
        />
        <v-row>
          <v-col cols="12">
            <v-text-field
              v-model="v$.firstName.$model"
              label="First Name"
              variant="solo"
              :error="v$.firstName.$error"
              :error-messages="errors.firstName"
            />
          </v-col>

          <v-col cols="12">
            <v-text-field
              v-model="v$.lastName.$model"
              label="Last Name"
              variant="solo"
              :error="v$.lastName.$error"
              :error-messages="errors.lastName"
            />
          </v-col>

          <v-col cols="12">
            <v-text-field
              v-model="v$.email.$model"
              label="E-mail"
              variant="solo"
              :error="v$.email.$error"
              :error-messages="errors.email"
            />
          </v-col>

          <v-col cols="12">
            <v-textarea
              v-model="v$.message.$model"
              label="Message"
              variant="solo"
              :error="v$.message.$error"
              :error-messages="errors.message"
            />
          </v-col>

          <v-col cols="12">
            <v-text-field
              v-model="v$.password.$model"
              label="Password"
              variant="solo"
              :error="v$.password.$error"
              :error-messages="errors.password"
            />
          </v-col>

          <v-col cols="12">
            <v-btn
              type="submit"
              color="success"
            >
              Submit
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    </main>
</template>

<script setup lang="ts">
import { useVuelidate } from '@vuelidate/core'

import { required, helpers, email } from '@vuelidate/validators'

import { FORM_INVALID_EMAIL, FORM_REQUIRED_FIELD } from '@/helpers/messages'

import type { IForm } from '@/interface/form.interface'

const loading = ref(false)

const form = ref<IForm>({
  firstName: '',
  lastName: '',
  email: '',
  message: '',
  password: ''
})

const rules = {
  firstName: {
    required: helpers.withMessage(FORM_REQUIRED_FIELD, required)
  },
  lastName: {
    required: helpers.withMessage(FORM_REQUIRED_FIELD, required)
  },
  email: {
    required: helpers.withMessage(FORM_REQUIRED_FIELD, required),
    email: helpers.withMessage(FORM_INVALID_EMAIL, email)
  },
  message: {
    required: helpers.withMessage(FORM_REQUIRED_FIELD, required)
  },
  password: {
    required: helpers.withMessage(FORM_REQUIRED_FIELD, required)
  }
}

const v$ = useVuelidate(rules, form)

const errors = computed(() => useValidationErrors<IForm>(v$.value.$errors))

const validate = async () => {
    const result = await v$.value.$validate() 

    if (!result) {
        return
    }

    loading.value = true

    setTimeout(() => {
      form.value = {
        firstName: '',
        lastName: '',
        email: '',
        message: '',
        password: ''
      }
      v$.value.$reset()
      loading.value = false
    }, 2500)
}
</script>

<style>
.form {
  margin: 100px auto 0 auto;
}
</style>