<template>
  <va-card class="col-span-12 sm:col-span-12">
    <va-card-title>Clients Management</va-card-title>
    <va-card-content>
      <form>
        <div class="grid grid-cols-12 gap-6">
          <div class="flex md:col-span-4 sm:col-span-6 col-span-12">
            <div class="flex md:col-span-4 sm:col-span-6 col-span-12">
              <va-input v-model="client_name" placeholder="Enter Client's Name" />
            </div>
          </div>
        </div>
      </form>
    </va-card-content>
    <va-card-content class="my-3 flex flex-wrap items-center gap-2">
      <va-button icon="add_circle_outline" @click="postUserScreenAccess()"> Save Record</va-button>
    </va-card-content>
  </va-card>
<br>
  <va-card class="col-span-12 sm:col-span-6 md:col-span-3" stripe stripe-color="info">
    <va-card-title>List of Clients</va-card-title>
    <va-card-content class="overflow-auto">
      <table class="va-table va-table--striped va-table--hoverable w-full">
        <thead>
          <tr>
            <th>Client's Name</th>
            <th>Status</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <td>X Sdn. Bhd.</td>
          <td>Active</td>
          <td v-if="client_name">Inactive</td>
          <td> <va-list-item-section icon>
              <va-icon name="edit" color="gray" />
            </va-list-item-section></td>
        </tbody>
      </table>
    </va-card-content>
  </va-card>

</template>

<script>
export default {

  data(){
    return{
      client_name: ""

    }
  }
}
</script>
