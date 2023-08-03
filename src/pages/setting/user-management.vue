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
            <va-checkbox v-model="checkbox.unselected1" label="Dashboard (m1)" value="m1" id="dashboard" @click="onClickScreenAccess('m1')"/>
            <va-checkbox v-model="checkbox.unselected2" label="Tender Management (m2)" value="m2" id="tender" @click="onClickScreenAccess('m2')"/>
            <va-checkbox v-model="checkbox.unselected3" label="CV Management (m3)" value="m3" id="cvmanagement" @click="onClickScreenAccess('m3')"/>
            <va-checkbox v-model="checkbox.unselected4" label="My Backup files (m4)" value="m4" id="backup" @click="onClickScreenAccess('m4')"/>
            <va-checkbox v-model="checkbox.unselected5" label="My CV (m5)" value="m5" id="mycv" @click="onClickScreenAccess('m5')"/>
            <va-checkbox v-model="checkbox.unselected6" label="Settings (m6)" value="m6" id="settings" @click="onClickScreenAccess('m6')"/>
          </fieldset>
        </div>
      </form>
    </va-card-content>
    <va-card-content class="my-3 flex flex-wrap items-center gap-2">
      <va-button icon="add_circle_outline" @click="onCreate()"> Save Record</va-button>
    </va-card-content>
  </va-card>
  <br />
  <va-card class="col-span-12 sm:col-span-6 md:col-span-3" stripe stripe-color="info">
    <va-card-title>List of Users</va-card-title>
    <va-card-content class="overflow-auto">
      <table class="va-table va-table--striped va-table--hoverable w-full">
        <thead>
          <tr>
            <th>Email</th>
            <th>Screen Access</th>
            <th>Status</th>
            <th><center>Action</center></th>
          </tr>
        </thead>

        <tbody>
          <tr  v-for="user in userlist">
            <td>
                      <a
                        style="color: #18846f; cursor: pointer"
                        >{{ user.email }}</a
                      >
            </td>
            <td>{{ user.user_access }}</td>
            <td v-if="user.status == 1">Active</td>  
            <td v-if="user.status == 0">Inactive</td>  
            <td> <va-list-item-section icon>
              <va-icon name="edit" color="gray" @click="editModal = !editModal"/>
              </va-list-item-section>
            </td>
          </tr>
        </tbody>
      </table>
    </va-card-content>
  </va-card>

  <!-- modal -->
  <va-modal
    v-slot="{ hide }"
    v-model="editModal"
    message=""
    hide-default-actions
    blur
  >
  <div class="flex flex-col items-start gap-2">
      <h3 class="va-h3">
        Edit User
      </h3>

      <div class="flex md:col-span-4 sm:col-span-6 col-span-12">
            <va-input v-model="modalEmail" placeholder="Enter Email (e.g: abc@araken.biz)" disabled/>
      </div>

      <div class="grid grid-cols-12 gap-6">
          <fieldset class="md:col-span-12 col-span-12 grid grid-cols-3 md:grid-cols-3 gap-3">
            <va-checkbox v-model="checkbox.unselected1" label="Dashboard (m1)" value="m1" id="dashboard" @click="onClickScreenAccess('m1')"/>
            <va-checkbox v-model="checkbox.unselected2" label="Tender Management (m2)" value="m2" id="tender" @click="onClickScreenAccess('m2')"/>
            <va-checkbox v-model="checkbox.unselected3" label="CV Management (m3)" value="m3" id="cvmanagement" @click="onClickScreenAccess('m3')"/>
            <va-checkbox v-model="checkbox.unselected4" label="My Backup files (m4)" value="m4" id="backup" @click="onClickScreenAccess('m4')"/>
            <va-checkbox v-model="checkbox.unselected5" label="My CV (m5)" value="m5" id="mycv" @click="onClickScreenAccess('m5')"/>
            <va-checkbox v-model="checkbox.unselected6" label="Settings (m6)" value="m6" id="settings" @click="onClickScreenAccess('m6')"/>
          </fieldset>
      </div>

    </div>

    <div class="flex justify-end mt-2 gap-2">
      <va-button
        preset="secondary"
        color="secondary"
        @click="hide()"
      >
        Cancel
      </va-button>
      <va-button
        @click="showSecondModal = !showSecondModal"
      >
        Save
      </va-button>
    </div>
  </va-modal>

  <!-- <va-modal
    v-model="showSecondModal"
    message="Are you sure you want to save it?"
    ok-text="Save"
    @ok="editModal = false"
  /> -->
</template>

<script>
import swal from 'sweetalert2';
export default {

  data() {

    return {
      userdetails: null,
      userlist : [],
      email: "",
      checkbox:  [
        {unselected1: false},
        {unselected2: false},
        {unselected3: false},
        {unselected4: false},
        {unselected5: false},
        {unselected6: false}
      ],
      screenaccess: "",
      editModal: false,
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
                          status: 1,
                        }, {headers}
                      );
                      console.log("response", response.data);
                      if (response.data.code == 200) {
                          //this.loader = false;
                          this.resetModel();
                          swal.fire('Record Successfully Saved', '', 'success')
                          this.GetUserDetails();
                      } else {
                          //this.loader = false;
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
      this.email='',
      this.checkbox='';
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
  },
}
</script>

<style scoped>
</style>
