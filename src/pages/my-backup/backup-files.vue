<template>
  <va-card class="col-span-12 sm:col-span-12">
    <va-card-title>My Backup Files</va-card-title>
    <va-card-content>
      <form>
        <div class="grid grid-cols-12 gap-6">
              <div class="flex md:col-span-3 sm:col-span-6 col-span-12">
                <va-input type="date" v-model="date_from" :max="this.date_to" placeholder="dd/mm/yyyy" label="FROM" id="dateFrom" @change="filterRows"/>
              </div>
              <div class="flex md:col-span-3 sm:col-span-6 col-span-12">
                <va-input type="date" v-model="date_to" :min="this.date_from" placeholder="dd/mm/yyyy" label="TO" id="dateTo" @change="filterRows()"/>
              </div>
                <div class="flex md:col-span-6 sm:col-span-8 col-span-12">
                  <va-input v-model="search" placeholder="Search by file name" id="searchFile" v-on:keyup="searchFileName()" label="SEARCH">
                  </va-input>
            </div>
          </div>
      </form>
    </va-card-content>
  </va-card>

  <br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                
  <va-card class="col-span-12 sm:col-span-6 md:col-span-3" stripe stripe-color="info">
    <va-card-title>List of Files</va-card-title>
    <va-card-content>
        <span style="display: flex;justify-content: right;"><va-file-upload v-model="backupFile" type="single" @change="uploadFile" v-if="!loader"></va-file-upload></span> 
      </va-card-content>
    <va-card-content class="overflow-auto">
      <table class="va-table va-table--striped va-table--hoverable w-full" id="fileList"> 
        <thead>
          <tr>
            <th>No</th>
            <th>Date Upload</th>
            <th>File Name</th>
            <th><center>Action</center></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(file, idx) in filelist" :key="idx">
            <td>{{ idx + 1 }}</td>
            <td>{{ getFormattedDate(file.created_at) }}</td>
            <td style="color: #0645AD;"><a v-bind:href=this.envURL+file.file_path target="_blank">{{ file.file_name }}</a></td>
            <td> <va-list-item-section icon><va-icon name="delete" style="color: rgb(168, 8, 8);" title="Delete Record"  @click="deleteRecord(file)"/></va-list-item-section></td>

          </tr>
        </tbody>
      </table>
    </va-card-content>
    <va-card-content>
      <Loader v-if="loader" />
    </va-card-content>
  </va-card>
</template>

<script >
import swal from 'sweetalert2';
import Loader from "../../components/loader.vue";
import moment from 'moment';
import $ from 'jquery';
export default{
  components: { Loader },
  data(){
    return{
      loader: false,
      userdetails: null,
      filelist: [],
      userid: "",
      email: "",
      search: "",
      date_from: "",
      date_to: "",
      backupFile: [],
      file:true,
      envURL: '',

    };
  },

  beforeMount(){
    this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
    this.userid = this.userdetails.user.user_id;
    this.email = this.userdetails.user.email;
    this.GetFileList();
  },

  mounted(){
  },

  methods: {
    async GetFileList(){
      const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "multipart/form-data",
        "Content-Type": "multipart/form-data",
      };
      const response = await this.$axios.get(
        this.userid + "/fileList",
        {headers}
      );

      if(response.data.code == 200 || response.data.code == '200'){
        this.filelist = response.data.list;
        this.envURL = response.data.envURL;
      }
    },

    getFormattedDate(date) {
            return moment(date).format("DD/MM/YYYY")
    },

    searchFileName(){
      var input, filter, table, tr, td, i , txtValue;

      input = document.getElementById("searchFile");
      filter = input.value.toUpperCase();
      table = document.getElementById("fileList");
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

      from = from || '1970-01-01'; // default from to a old date if it is not set
      to = to || '2999-12-31';

      var dateFrom = moment(from).format("YYYY-MM-DD");
      var dateTo = moment(to).format("YYYY-MM-DD");

      $('#fileList tr:not(:first)').each(function(i, tr) {
        var val = $(tr).find("td:nth-child(2)").text();
        var dateVal = moment(val, "DD/MM/YYYY");
        var visible = (dateVal.isBetween(dateFrom, dateTo, null, [])) ? "" : "none"; // [] for inclusive
        $(tr).css('display', visible);
      });
      
    },

    async uploadFile(){
      try{
        this.loader = true;
        const headers = {
          Authorization: "Bearer " + this.userdetails.authorization.token,
          Accept: "application/json",
          "Content-Type": "application/json",
        };

        let body = new FormData();

        body.append("user_id", this.userid);
        body.append("file", this.backupFile);
        body.append("email", this.email);

        const response = await this.$axios.post(
          "fileStore",
          body,{headers}
        );
        
        if(response.data.code == 200 || response.data.code == '200'){
          this.loader = false;
          swal.fire('Record Successfully Saved', '', 'success')
          this.GetFileList();
          this.backupFile=[];
          this.file=true;
        }
        else {
                this.loader = false;
                  swal.fire({
                      icon: 'error',
                      title: 'Oops... Something Went Wrong!',
                      text: 'the error is: ' + JSON.stringify(response.data.message),
                  })
          
              }
      }catch(e){
        swal.fire({
                    icon: 'error',
                    title: 'Oops... Something Went Wrong!',
                    text: 'the error is: ' + e,
                })
      }
    },
    async deleteRecord(data){
      swal.fire({
                  title: 'Are you sure to delete this record?',
                  showCancelButton: true,
                  confirmButtonText: 'Delete',
                }).then(async (result) => {
                if (result.isConfirmed) {
                  try {
                    this.loader = true;
                    const headers = {
                            Authorization: "Bearer " + this.userdetails.access_token,
                            Accept: "application/json",
                            "Content-Type": "application/json",
                          };
                          const response = await this.$axios.post(
                            "deleteFile",{ 
                              backup_id: data.backup_id,
                              file_path: data.file_path
                            },
                            { headers }
                          );
                          if (response.data.code == 200 || response.data.code == '200') {
                            this.GetFileList();
                            this.loader = false;
                              swal.fire('Record Successfully Deleted', '', 'success')
                          } else {
                            this.loader = false;
                              swal.fire({
                                  icon: 'error',
                                  title: 'Oops... Something Went Wrong!',
                                  text: 'the error is: ' + JSON.stringify(response.data.message),
                              })
                      
                          }
                  } catch (e) {
                    this.loader = false;
                          swal.fire({
                              icon: 'error',
                              title: 'Oops... Something Went Wrong!',
                              text: 'the error is: ' + e,
                          })
                      }
                } else if (result.isDismissed) {
                      swal.fire('Record are not saved', '', 'info')
                  }
              })

    },
  }

}

</script>

<style scoped></style>
