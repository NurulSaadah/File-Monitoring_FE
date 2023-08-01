<template>
  <va-card class="col-span-12 sm:col-span-6 md:col-span-3" stripe stripe-color="info">
    <va-card-title>List of Tenders</va-card-title>
    <va-card-content class="overflow-auto">
      <span style="float: right"><va-button icon="add_circle_outline" type="submit" @click="onAdd()"><Loader v-if="loader" />Add New Record</va-button></span>
      <table class="va-table va-table--striped va-table--hoverable w-full mt-6">
        <thead>
          <tr>
            <th>No</th>
            <th>Submission Date</th>
            <th>Title</th>
            <th>Reference No.</th>
            <th>Submission Price (RM)</th>
            <th style="width: 5px;">Action</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(clt, idx) in tenderList" :key="idx">
            <td>{{ idx + 1 }}</td>
            <td>{{ clt.submission_date }}</td>
            <td>{{ clt.title }}</td>
            <td>{{ clt.reference_no }}</td>
            <td>{{ clt.submission_price }}</td>
            <td> <va-list-item-section icon><va-icon name="eye" title="View Record" color="success" @click="viewRecord(clt)" /><va-icon name="edit" title="Edit Record" color="gray" @click="editRecord(clt)" /></va-list-item-section></td>
          </tr>
        </tbody>
      </table>
      <p v-show="!tenderList.length" style=" padding: 0px; margin: 10px;color: red;display: flex;justify-content: center;">No Record Found</p>
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
      tenderList: [],
      
    };
  },
  mounted() {
   

  },
  beforeMount() {
    
    this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
    this.GetTenderList();
    
  },
  methods: {
    async GetTenderList(){
      const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "tenderList",
        {headers}
      );

      if(response.data.code == 200){
        this.tenderList = response.data.list;
      }
    },
    
  }
};

</script>

<style scoped></style>
