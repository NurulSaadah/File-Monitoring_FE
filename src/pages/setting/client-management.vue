<template>
  <va-card class="col-span-12 sm:col-span-12">
    <va-card-title>Setting : Client Management</va-card-title>
    <va-card-content>
      <form>
        <div class="grid grid-cols-12 gap-6">
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <va-input v-model="clientName" placeholder="Enter Client Name" />
          </div>
          <div class="flex md:col-span-2 sm:col-span-6 col-span-12">
            <select style="border-radius: 5px; border: 1px solid lightgrey;width: 100%;height: 100%;"
                  v-model="status"
                  class="form-select"
                  aria-label="Default select example" label="Status">
                    <option value="" disabled selected>Select Status</option>
                    <option 
                      v-for="stat in statusList"
                        v-bind:key="stat.setting_id"
                        v-bind:value="stat.setting_id">
                      {{ stat.parameter }}</option>

                 </select>
          </div>
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <input type="hidden" v-model="editID" />
          </div>
        </div>
       
      </form>
    </va-card-content>
    <va-card-content class="my-3 flex flex-wrap items-center gap-2">
      <va-button icon="add_circle_outline" type="submit" @click="onCreate()" color="success"><Loader v-if="loader" />Save Record</va-button>
    </va-card-content>
  </va-card>
  <br />
  <va-card class="col-span-12 sm:col-span-6 md:col-span-3" stripe stripe-color="info">
    <va-card-title>List of Clients</va-card-title>
  
     <div class="flex md:col-span-2 sm:col-span-6 col-span-12 ml-5">
            <select style="border-radius: 5px; border: 1px solid lightgrey;width: 30%;height: 150%;"
                  v-model="typeSearch"
                  class="form-select"
                  aria-label="Default select example" label="Status"
                  @change="onChange($event)">>
                    <option value="" disabled selected>Select Status</option>
                    <option 
                      v-for="stat in statusList"
                        v-bind:key="stat.setting_id"
                        v-bind:value="stat.setting_id">
                      {{ stat.parameter }}</option>

                 </select>
          </div>
    <va-card-content class="overflow-auto">
      <table class="va-table va-table--striped va-table--hoverable w-full">
        <thead>
          <tr>
            <th>No</th>
            <th>Client</th>
            <th>Status</th>
            <th style="width: 5px;">Action</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(clt, idx) in clientList" :key="idx">
            <td>{{ idx + 1 }}</td>
            <td>{{ clt.client_name }}</td>
            <td v-if="clt.client_status == 1">Active</td>  
            <td v-if="clt.client_status == 0">Inactive</td>  
            <td> <va-list-item-section icon><va-icon name="edit" title="Edit Record" color="gray" @click="editRecord(clt)" /></va-list-item-section></td>
          </tr>
        </tbody>
      </table>
      <p v-show="!clientList.length" style=" padding: 0px; margin: 10px;color: red;display: flex;justify-content: center;">No Record Found</p>
    </va-card-content>
  </va-card>
</template>

<script>
import swal from 'sweetalert2';
import Loader from "../../components/loader.vue";
export default {
  components: { Loader },
  data() {
    return {
      loader:false,
      clientList: [],
      statusList:[],
      status:'',
      typeSearch:'ALL',
      clientName:'',
      clientStatus:'',
      editID:'',
    };
  },
  mounted() {
   

  },
  beforeMount() {
    
    this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
    this.GetClientList();
    this.GetList();
    
  },
  methods: {
    async GetClientList(){
      const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "clientList",
        {headers}
      );

      if(response.data.code == 200){
        this.clientList = response.data.list;
      }
    },
    
    async onChange(event) {
      if(event.target.value!='ALL'){
      const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        event.target.value + "/statusSearchList",
        {headers}
      );

      if(response.data.code == 200){
        this.clientList = response.data.list;
      }
    }else{
      this.GetClientList();
    }
    },
    //add new record
    async onCreate() {
      swal.fire({
                  title: 'Do you want save this record?',
                  showCancelButton: true,
                  confirmButtonText: 'Save',
                }).then(async (result) => {
                if (result.isConfirmed) {
                  try {
                    this.loader = true;
                          const response = await this.$axios.post(
                              "clientStore", {
                                 client_name:this.clientName,
                                 client_status:this.status,
                                  editId:this.editID,
                                
                              }, 
                          );
                          console.log("response", response.data);
                          if (response.data.code == 200) {
                            this.loader = false;
                              this.resetModel();
                              swal.fire('Record Successfully Saved', '', 'success')
                              this.GetClientList();
                          } else {
                            this.loader = false;
                              this.resetModel();
                              swal.fire({
                                  icon: 'error',
                                  title: 'Oops... Something Went Wrong!',
                                  text: 'the error is: ' + JSON.stringify(response.data.message),
                              })
                      
                          }
                  } catch (e) {
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
    resetModel(){
      this.clientName='',
      this.status='',
      this.editID=''
    },
    //edit record
    //get record to update
    async editRecord(data){
     this.editID=data.client_id;
     this.clientName=data.client_name;
     this.status=data.client_status;
    },

    async GetList(){
      const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "status" + "/typeSearchList",
        {headers}
      );

      if(response.data.code == 200){
        this.statusList = response.data.list;
      }
    },

   
     
   

  }
};

</script>

<style scoped></style>
