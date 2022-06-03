<script setup lang="ts">
import { computed, reactive } from 'vue';
import { Form } from 'ant-design-vue';
import type { RuleItem } from 'async-validator';

const form = reactive({
  name: '',
  lastName: ''
})
const rules: Record<string, RuleItem[]> = reactive({
  name: [
    {
      required: false,
    },
  ],
  lastName: [
    {
      required: false,
    },
  ]
})

const { validate, validateInfos } = Form.useForm(form, rules)

const isFormValid = computed(() => {
  // validateInfos is not actual on init. It does not have neede properties to calculate whether the form is valid initially or not

  return Object.keys(validateInfos)
    .map((key) => validateInfos[key])
    .every((info) => info.validateStatus === "success")
})

// workaround: to call `validate` method at once (uncomment this line to test):
// validate() 
</script>

<template>
  {{ isFormValid }}
  <AForm :model="form">
    <AFormItem has-feedback label="Name" name="name">
      <AInput v-model:value="form.name" />
    </AFormItem>

    <AFormItem has-feedback label="Last Name" name="lastName">
      <AInput v-model:value="form.lastName" />
    </AFormItem>

    <AButton :disabled="!isFormValid">
      Submit
    </AButton>
  </AForm>
</template>
