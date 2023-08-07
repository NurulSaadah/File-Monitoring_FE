<template>
  <va-card class="col-span-12 sm:col-span-12">
    <va-card-title>Setting : General Setting</va-card-title>
    <va-card-content>
      <form>
        <div class="grid grid-cols-12 gap-6">
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <va-input v-model="type" placeholder="Enter Type" />
          </div>
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <va-input v-model="parameter" placeholder="Enter Parameter" />
          </div>
          <div class="flex md:col-span-2 sm:col-span-6 col-span-12">
            <va-input v-model="value" placeholder="Enter Value" />
          </div>
          <div class="flex md:col-span-2 sm:col-span-6 col-span-12">
            <va-input v-model="code" placeholder="Enter Code" />
          </div>
          <div class="flex md:col-span-2 sm:col-span-6 col-span-12">
            <va-input v-model="index" placeholder="Enter Index" />
          </div>
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <va-input v-model="description" placeholder="Enter Description" />
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
    <va-card-title>List of Settings</va-card-title>
    <div class="grid grid-cols-12 gap-6 ml-5">
            <div class="flex md:col-span-12 col-span-12">
              <select style="border-radius: 5px; border: 1px solid lightgrey;width: 30%;height: 130%;"
                  v-model="typeSearch"
                  class="form-select"
                  aria-label="Default select example"
                  @change="onChange($event)">
                  
                    <option value="ALL">All List of Type</option>
                    <option
                      v-for="typ in typeList"
                      v-bind:key="typ.type"
                      v-bind:value="typ.type"
                    >
                  {{ typ.type }}
                </option>
                 </select>
             
            </div>
     </div>

    <va-card-content class="overflow-auto">
      <table class="va-table va-table--striped va-table--hoverable w-full">
        <thead>
          <tr>
            <th>No</th>
            <th>Type</th>
            <th>Parameter</th>
            <th>Value</th>
            <th>Code</th>
            <th>Index</th>
            <th>Description</th>
            <th style="width: 5px;">Action</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(set, idx) in settingList" :key="idx">
            <td>{{ idx + 1 }}</td>
            <td>{{ set.type }}</td>
            <td>{{ set.parameter }}</td>
            <td>{{ set.value }}</td>
            <td>{{ set.code }}</td>
            <td>{{ set.index }}</td>
            <td>{{ set.description }}</td>
            <td> <va-list-item-section icon><va-icon name="delete" style="color: rgb(168, 8, 8);" title="Delete Record"  @click="deleteRecord(set)" /> <va-icon name="edit" title="Edit Record" color="gray" @click="editRecord(set)" /></va-list-item-section></td>
          </tr>
        </tbody>
      </table>
      <p v-show="!settingList.length" style=" padding: 0px; margin: 10px;color: red;display: flex;justify-content: center;">No Record Found</p>
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
      settingList: [],
      typeList: [],
      statuslist: [],
      typeSearch:'ALL',
      type:'',
      parameter:'',
      value:'',
      code:'',
      index:'',
      description:'',
      editID:'',
      
    };
  },
  mounted() {
   

  },
  beforeMount() {
    
    this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
    this.GetSettingList();
    this.GetTypeList();
    
  },
  methods: {
    async GetSettingList(){
      const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "settingList",
        {headers}
      );

      if(response.data.code == 200){
        this.settingList = response.data.list;
      }
    },
    async GetTypeList(){
      const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "typeList",
        {headers}
      );

      if(response.data.code == 200){
        this.typeList = response.data.list;
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
        event.target.value + "/typeSearchList",
        {headers}
      );

      if(response.data.code == 200){
        this.settingList = response.data.list;
      }
    }else{
      this.GetSettingList();
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
                              "settingStore", {
                                  type: this.type,
                                  parameter:this.parameter,
                                  value:this.value,
                                  code:this.code,
                                  index:this.index,
                                  description:this.description,
                                  editId:this.editID,
                                
                              }, 
                          );
                          console.log("response", response.data);
                          if (response.data.code == 200) {
                            this.loader = false;
                              this.resetModel();
                              swal.fire('Record Successfully Saved', '', 'success')
                              this.GetSettingList();
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
      this.type='',
      this.parameter='',
      this.value='',
      this.code='',
      this.index='',
      this.description='',
      this.editID
    },
    //edit record
    //get record to update
    async editRecord(data){
     this.editID=data.setting_id;
     this.type=data.type;
     this.parameter=data.parameter;
     this.value=data.value;
     this.code=data.code;
     this.index=data.index;
     this.description=data.description;
    },

    //delete record
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
                            "deleteSetting",{ id: data.setting_id },
                            { headers }
                          );
                          if (response.data.code == 200) {
                            this.loader = false;
                              this.resetModel();
                              swal.fire('Record Successfully Deleted', '', 'success')
                              this.GetSettingList();
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

    }
     
   

  }
};

</script>

<style scoped></style>
