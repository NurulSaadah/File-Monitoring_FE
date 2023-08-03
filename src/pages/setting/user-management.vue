<template>
  <va-card class="col-span-12 sm:col-span-12">
    <va-card-title>Setting : User Management</va-card-title>
    <va-card-content>
      <form>
        <div class="grid grid-cols-12 gap-6">
          <div class="flex md:col-span-4 sm:col-span-6 col-span-12">
            <va-input v-model="email" placeholder="Enter Email (e.g: abc@araken.biz)" />
          </div>
        </div>
        <div class="mt-5">Screen Access :</div>
        <br />
        <div class="grid grid-cols-12 gap-6">
          <fieldset class="md:col-span-12 col-span-12 grid grid-cols-3 md:grid-cols-3 gap-3">
            <va-checkbox v-model="m1" label="Dashboard (m1)" value="m1" id="dashboard" @click="onClickScreenAccess('m1')"/>
            <va-checkbox v-model="m2" label="Tender Management (m2)" value="m2" id="tender" @click="onClickScreenAccess('m2')"/>
            <va-checkbox v-model="m3" label="CV Management (m3)" value="m3" id="cvmanagement" @click="onClickScreenAccess('m3')"/>
            <va-checkbox v-model="m4" label="My Backup files (m4)" value="m4" id="backup" @click="onClickScreenAccess('m4')"/>
            <va-checkbox v-model="m5" label="My CV (m5)" value="m5" id="mycv" @click="onClickScreenAccess('m5')"/>
            <va-checkbox v-model="m6" label="Settings (m6)" value="m6" id="settings" @click="onClickScreenAccess('m6')"/>
          </fieldset>
        </div>
      </form>
      <div class="mt-5"></div>
      <div class="grid grid-cols-12 gap-6">
          <div class="flex md:col-span-4 sm:col-span-6 col-span-12">
            <select style="border-radius: 5px; border: 1px solid lightgrey;width: 30%;height: 130%;"
                  v-model="userStatus"
                  class="form-select"
                  aria-label="Default select example" label="Status">
                    <option value="" disabled selected>Status</option>
                    <option value="1">Active</option>
                    <option value="0">Inactive</option>

                 </select>
          </div>
        </div>
    </va-card-content>
    <va-card-content class="my-3 flex flex-wrap items-center gap-2">
      <va-button icon="add_circle_outline" @click="onCreate()"><Loader v-if="loader" /> Save Record</va-button>
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
            <th>Email</th>
            <th>Screen Access</th>
            <th>Status</th>
            <th><center>Action</center></th>
          </tr>
        </thead>

        <tbody>
          <tr  v-for="(user, idx) in userlist" :key="idx">
            <td>{{ idx + 1 }}</td>
            <td>
                      <a
                        style="color: #18846f; cursor: default " 
                        >{{ user.email }}</a
                      >
            </td>
            <td>{{ user.user_access }}</td>
            <td v-if="user.status == 1">Active</td>  
            <td v-if="user.status == 0">Inactive</td>  
            <td> <va-list-item-section icon>
              <va-icon name="edit" color="gray" @click="editRecord(user)"/>
              </va-list-item-section>
            </td>
          </tr>
        </tbody>
      </table>
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
      userdetails: null,
      userlist : [],
      email: "",
      m1: false,
      m2: false,
      m3: false,
      m4: false,
      m5: false,
      m6: false,
      userStatus: "",
      screenaccess: "",
      showSecondModal: false,
      modalEmail: "",
    };

  },

  beforeMount(){ 
    this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
    this.GetUserDetails();
  },

  mounted(){
  },                                                

  methods : {
    async GetUserDetails(){
      const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "/user/getUserList",
        {headers}
      );

      if(response.data.code == 200 || response.data.code == '200'){
        this.userlist = response.data.list;
      }
    },

    async onCreate() {
      swal.fire({
                  title: 'Do you want save this record?',
                  showCancelButton: true,
                  confirmButtonText: 'Save',
                }).then(async (result) => {
                if (result.isConfirmed) {
                  try {
                    this.loader = true;
                    const headers = {
                      Authorization: "Bearer " + this.userdetails.authorization.token,
                      Accept: "application/json",
                      "Content-Type": "application/json",
                    };
                    const response = await this.$axios.post(
                        "/user/insertOrupdate",
                        {
                          email: this.email,
                          user_access: this.screenaccess,
                          status: this.userStatus,
                        }, {headers}
                      );
                      console.log("response", response.data);
                      if (response.data.code == 200) {
                          this.loader = false;
                          this.resetModel();
                          swal.fire('Record Successfully Saved', '', 'success')
                          this.GetUserDetails();
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
      this.email='';
      this.m1 = false;
      this.m2 = false;
      this.m3 = false;
      this.m4 = false;
      this.m5 = false;
      this.m6 = false;
      this.screenaccess = "";
      this.userStatus = "";
    },

    onClickScreenAccess(val){
      var sa = this.screenaccess;
      var checksum; //to check if the variable already contains new value.
      var commacheckleft;
      var commacheckright;

      checksum = sa.includes(val);

      if(sa){

        if(checksum){
          function deleteSameValue(sa, val) { //incase of deselect of a value
            commacheckleft = sa.includes(','+ val);
            commacheckright = sa.includes(val+ ',');
            if(commacheckleft){
              const regex = new RegExp(','+val, 'g'); //g stands for global.
              return sa.replace(regex, '');
            }else if(commacheckright){  
              const regex = new RegExp(val+',', 'g');
              return sa.replace(regex, '');
            }else{
              const regex = new RegExp(val, 'g');
              return sa.replace(regex, '');
            }
          };

          this.screenaccess = deleteSameValue(sa, val);
        }else{
          this.screenaccess = this.screenaccess + "," + val;
        }
      }
      else{
        this.screenaccess = val;
      };

      this.screenaccess = this.filterAlphanumerically(this.screenaccess);

    },

    filterAlphanumerically(inputString) {
      // Step 1: Split the inputString into an array of values
      const valuesArray = inputString.split(',');

      // Step 2: Sort the array alphanumerically
      valuesArray.sort(function(a, b) {
        return a.localeCompare(b, undefined, { numeric: true, sensitivity: 'base' });
      });

      // Step 3: Join the sorted array back into a string
      const sortedString = valuesArray.join(',');

      return sortedString;
    },

    editRecord(data){
      this.resetModel();
      var mArray;

      this.email = data.email;
      this.userStatus = data.status;

      mArray = data.user_access.split(',');

      for(let i = 0; i< mArray.length; i++){
        if(mArray[i] == 'm1'){
          this.m1 = true
        }
        if(mArray[i] == 'm2'){
          this.m2 = true
        }
        if(mArray[i] == 'm3'){
          this.m3 = true
        }
        if(mArray[i] == 'm4'){
          this.m4 = true
        }
        if(mArray[i] == 'm5'){
          this.m5 = true
        }
        if(mArray[i] == 'm6'){
          this.m6 = true
        };
      };

      this.screenaccess = data.user_access;

      this.scrollToTop();

    },

    scrollToTop() {
      window.scrollTo({
          top: 0,
          behavior: "smooth"
        });
    }
  },
}
</script>

<style scoped>
</style>
