<template>
  <va-card class="col-span-12 sm:col-span-12">
    <va-card-title>Users Management</va-card-title>
    <va-card-content>
      <form>
        <div class="grid grid-cols-12 gap-6">
          <div class="flex md:col-span-4 sm:col-span-6 col-span-12">
            <va-input v-model="simple" placeholder="Enter Email (e.g: abc@araken.biz)" />
          </div>
        </div>
        <div class="mt-5">Screen Access :</div>
        <br />
        <div class="grid grid-cols-12 gap-6">
          <fieldset class="md:col-span-8 col-span-12 grid grid-cols-3 md:grid-cols-3 gap-3">
            <va-checkbox v-model="checkbox.unselected1" :label="t('forms.controls.dashboardPage')" />
            <va-checkbox v-model="checkbox.unselected2" :label="t('forms.controls.tenderPage')" />
            <va-checkbox v-model="checkbox.unselected3" :label="t('forms.controls.cvPage')" />
            <va-checkbox v-model="checkbox.unselected4" :label="t('forms.controls.backupPage')" />
            <va-checkbox v-model="checkbox.unselected5" :label="t('forms.controls.myCvPage')" />
            <va-checkbox v-model="checkbox.unselected6" :label="t('forms.controls.settingPage')" />
          </fieldset>
        </div>
      </form>
    </va-card-content>
    <va-card-content class="my-3 flex flex-wrap items-center gap-2">
      <va-button icon="add_circle_outline"> Save Record</va-button>
    </va-card-content>
  </va-card>
  <br />
  <va-card class="col-span-12 sm:col-span-6 md:col-span-3" stripe stripe-color="info">
    <va-card-title>List of Users</va-card-title>
    <va-card-content>{{ t('cards.contentTextLong') }}</va-card-content>
  </va-card>
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import { useI18n } from 'vue-i18n'
  const { t } = useI18n()

  const simple = ref('')
  const checkbox = ref({
    unselected1: false,
    unselected2: false,
    unselected3: false,
    unselected4: false,
    unselected5: false,
    unselected6: false,
  })
</script>

<style scoped></style>
