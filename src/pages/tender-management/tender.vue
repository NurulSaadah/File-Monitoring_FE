<template>
  <va-card class="col-span-12 sm:col-span-12">
    <va-card-title>Tender Management : List of Tenders</va-card-title>
    <va-card-content>
      <form>
        <div class="grid grid-cols-12 gap-6">
              <div class="flex md:col-span-3 sm:col-span-6 col-span-12">
                <va-input type="date" v-model="date_from" placeholder="dd/mm/yyyy" label="FROM" id="dateFrom" @change="filterRows()"/>
              </div>
              <div class="flex md:col-span-3 sm:col-span-6 col-span-12">
                <va-input type="date" v-model="date_to" placeholder="dd/mm/yyyy" label="TO" id="dateTo" @change="filterRows()"/>
              </div>
                <div class="flex md:col-span-6 sm:col-span-8 col-span-12">
                  <va-input v-model="search" placeholder="Search by title" id="searchFile" v-on:keyup="searchFileName()" label="SEARCH">
                  </va-input>
            </div>
          </div>
      </form>
    </va-card-content>
  </va-card>

  <br>
  <va-card class="col-span-12 sm:col-span-6 md:col-span-3" stripe stripe-color="info">
    
    <va-card-content class="overflow-auto">
      <span style="float: right"><va-button icon="add_circle_outline" type="submit" @click="onAdd()"><Loader v-if="loader" />Add New Record</va-button></span>
      <table class="va-table va-table--striped va-table--hoverable w-full mt-10" id="tenderList">
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
import moment from 'moment';
export default {
  components: { Loader },
  data() {
    return {
      loader:false,
      tenderList: [],
      search: "",
      date_from: "",
      date_to: "",
      
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

    async onAdd(){
      this.$router.push({ name: 'new-tender' });
    },
    async viewRecord(){
      this.$router.push({ name: 'view-tender' });
    },
    async editRecord(){
      this.$router.push({ name: 'edit-tender' });
    },

    getFormattedDate(date) {
            return moment(date).format("DD/MM/YYYY")
    },

    searchFileName(){
      var input, filter, table, tr, td, i , txtValue;

      input = document.getElementById("searchFile");
      filter = input.value.toUpperCase();
      table = document.getElementById("tenderList");
      tr = table.getElementsByTagName("tr");
      for (i = 0; i < tr.length; i++) {
        td = tr[i].getElementsByTagName("td")[2];             //2 indicates the third column
        if (td) {
          txtValue = td.textContent || td.innerText;
          if (txtValue.toUpperCase().indexOf(filter) > -1) {
            tr[i].style.display = "";
          } else {
            tr[i].style.display = "none";
          }
        }       
      }
    },

    filterRows() {
      var from = $('#dateFrom').val();
      var to = $('#dateTo').val();
      
      if (!from && !to) { // no value for from and to
        return;
      }

      from = from || '1970-01-01'; // default from to a old date if it is not set
      to = to || '2999-12-31';

      var dateFrom = moment(from);
      var dateTo = moment(to);

      $('#filelist tr').each(function(i, tr) {
        var val = $(tr).find("td:nth-child(2)").text();
        var dateVal = moment(val, "DD/MM/YYYY");
        var visible = (dateVal.isBetween(dateFrom, dateTo, null, [])) ? "" : "none"; // [] for inclusive
        $(tr).css('display', visible);
      });
    },
    
  }
};

</script>

<style scoped></style>
