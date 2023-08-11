<template>
  <va-card class="col-span-12 sm:col-span-6 md:col-span-3" stripe stripe-color="info">
    <va-card-title>CV Management : List of CV</va-card-title>
    <va-card-content class="overflow-auto">
      <table class="va-table va-table--striped va-table--hoverable w-full mt-10">
        <thead>
          <tr>
            <th>No</th>
            <th>Email</th>
            <th style="width: 5px;">Action</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(user, idx) in cvList" :key="idx">
            <td>{{ idx + 1 }}</td>
            <td>{{ user.email }}</td>
            <td><va-list-item-section icon><va-icon name="eye" title="View Record" color="success" @click="viewRecord(user)" /></va-list-item-section></td>
          </tr>
        </tbody>
      </table>
      <p v-show="!cvList.length" style=" padding: 0px; margin: 10px;color: red;display: flex;justify-content: center;">No Record Found</p>
    </va-card-content>
  </va-card>
</template>

<script>
import Loader from "../../components/loader.vue";
export default {
  components: { Loader },
  data() {
    return {
      loader:false,
      cvList: [],

    };
  },
  mounted() {


  },
  beforeMount() {

    this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
    this.GetCVList();

  },
  methods: {
    async GetCVList(){
    },

  }
};

</script>

<style scoped></style>
