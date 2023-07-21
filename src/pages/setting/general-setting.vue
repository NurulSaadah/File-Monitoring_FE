<template>
  <va-card class="col-span-12 sm:col-span-12">
    <va-card-title>Users Management</va-card-title>
    <va-card-content>
      <form>
        <div class="grid grid-cols-12 gap-6">
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <va-input v-model="type" placeholder="Enter Type" />
          </div>
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <va-input v-model="parameter" placeholder="Enter Parameter" />
          </div>
          <div class="flex md:col-span-1 sm:col-span-6 col-span-12">
            <va-input v-model="value" placeholder="Enter Value" />
          </div>
          <div class="flex md:col-span-2 sm:col-span-6 col-span-12">
            <va-input v-model="code" placeholder="Enter Code" />
          </div>
          <div class="flex md:col-span-1 sm:col-span-6 col-span-12">
            <va-input v-model="index" placeholder="Enter Index" />
          </div>
          <div class="flex md:col-span-8 sm:col-span-6 col-span-12">
            <va-input v-model="description" placeholder="Enter Description" />
          </div>
        </div>
       
      </form>
    </va-card-content>
    <va-card-content class="my-3 flex flex-wrap items-center gap-2">
      <va-button icon="add_circle_outline" type="submit" @click="onCreate()"> Save Record</va-button>
    </va-card-content>
  </va-card>
  <br />
  <va-card class="col-span-12 sm:col-span-6 md:col-span-3" stripe stripe-color="info">
    <va-card-title>List of Users</va-card-title>
    <va-card-content class="overflow-auto">
      <table class="va-table va-table--striped va-table--hoverable w-full">
        <thead>
          <tr>
            <th>No</th>
            <th>Category</th>
            <th>Parameter</th>
            <th>Value</th>
            <th>Code</th>
            <th>Index</th>
            <th>Description</th>
            <th>Action</th>
          </tr>
        </thead>

        <tbody>
          <tr>
            <td>1</td>
            <td>Associate</td>
            <td>position</td>
            <td>-</td>
            <td>-</td>
            <td>1</td>
            <td>Position Avalaible in Araken</td>
            <td> <va-list-item-section icon>
              <va-icon name="edit" color="gray" />
            </va-list-item-section></td>
          </tr>
          <tr>
            <td>2</td>
            <td>Sr Associate</td>
            <td>position</td>
            <td>-</td>
            <td>-</td>
            <td>2</td>
            <td>Position Avalaible in Araken</td>
            <td> <va-list-item-section icon>
              <va-icon name="edit" color="gray" />
            </va-list-item-section></td>
          </tr>
        </tbody>
      </table>
    </va-card-content>
  </va-card>
</template>

<script>
import swal from 'sweetalert2';
export default {

  data() {
    return {
      simple:'',
    };
  },
  mounted() {
  

  },
  beforeMount() {
   
  },
  methods: {
    async onCreate() {
      swal.fire({
                  title: 'Do you want save this record?',
                  showCancelButton: true,
                  confirmButtonText: 'Save',
              }).then(async (result) => {

                if (result.isConfirmed) {
                } else if (result.isDismissed) {
                      this.$swal.fire('Record are not saved', '', 'info')
                  }
              })
    },
       
  }
};

</script>

<style scoped></style>
