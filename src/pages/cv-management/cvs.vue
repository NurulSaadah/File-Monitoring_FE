<template>
  <va-card class="col-span-12 sm:col-span-6 md:col-span-3">
    <va-card-title>CV Management : List of CV</va-card-title>
    <va-card-content>
      <form>
        <div class="grid grid-cols-12 gap-6">
                <div class="flex md:col-span-6 sm:col-span-8 col-span-12">
                  <va-input v-model="search" placeholder="Search by email" id="searchFile" v-on:keyup="searchFileName()" label="SEARCH">
                  </va-input>
            </div>
          </div>
      </form>
    </va-card-content>
  </va-card>
  <br>
  <va-card class="col-span-12 sm:col-span-6 md:col-span-3" stripe stripe-color="info">
    <va-card-content class="overflow-auto">
      <table class="va-table va-table--striped va-table--hoverable w-full mt-10" id="cvlist">
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
      const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "getUserList",
        {headers}
      );

      if(response.data.code == 200 || response.data.code == '200'){
        this.cvList = response.data.list;
      }
    },

    searchFileName(){
      var input, filter, table, tr, td, i , txtValue;

      input = document.getElementById("searchFile");
      filter = input.value.toUpperCase();
      table = document.getElementById("cvlist");
      tr = table.getElementsByTagName("tr");
      for (i = 0; i < tr.length; i++) {
        td = tr[i].getElementsByTagName("td")[1];             //2 indicates the third column
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

    async viewRecord(){
      this.$router.push({ name: 'view-cv' });
    },

  }
};

</script>

<style scoped></style>
