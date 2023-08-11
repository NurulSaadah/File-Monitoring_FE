<template>
  <va-card class="col-span-12 sm:col-span-12">
    <va-card-title>Tender Management : New Record</va-card-title>
    <va-card-content>
      <form>
        <div class="grid grid-cols-12 gap-6">
          <div class="flex md:col-span-12 sm:col-span-6 col-span-12">
            <va-input v-model="title" placeholder="Enter Tender Title" label="title" />
          </div>
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <select style="border-radius: 5px; border: 1px solid lightgrey;width: 100%;height: 100%;"
                  v-model="client"
                  class="form-select"
                  aria-label="Default select example" label="Status">
                    <option value="" disabled selected>Select Client</option>
                    <option 
                      v-for="clt in clientList"
                        v-bind:key="clt.client_id"
                        v-bind:value="clt.client_id">
                      {{ clt.client_name }}</option>

                 </select>
          </div>
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <va-input v-model="refNo" placeholder="Enter Tender Reference No." label="Reference Number" />
          </div>
          <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <va-input type="date" v-model="date" placeholder="dd/mm/yyyy" label="FROM" id="dateFrom" />
            </div>
            <div class="flex md:col-span-6 sm:col-span-6 col-span-12">
            <va-input v-model="price" placeholder="Enter Submission Price (0.00)" label="Submission Price" />
          </div>
          <div class="flex md:col-span-12 sm:col-span-6 col-span-12">
            <va-input type="textarea" v-model="remark" placeholder="Enter Remark" label="remark" rows="10" />
          </div>
         
        </div>
       
      </form>
    </va-card-content>

  </va-card>
  <br>
  <va-card class="col-span-2">
    <va-card-title>Upload Document</va-card-title>
    <div class="grid grid-cols-12 gap-6">
      <div class="flex md:col-span-12 sm:col-span-6 col-span-12 ml-5">
        <va-file-upload v-model="tenderRequirementFile" type="single">
          1. <span style="color: blue;text-decoration:underline;">Tender Requirement</span>
        </va-file-upload>
      </div>

    </div>
    <va-card-title>Tender Submission</va-card-title>
    <div class="grid grid-cols-12 gap-6">
      <div class="flex md:col-span-4 sm:col-span-6 col-span-12 ml-5">
        <va-file-upload v-model="technicalSubmissionFile" type="single">
          1. <span style="color: blue;text-decoration:underline;">Technical Submission</span>
        </va-file-upload>
      </div>
      <div class="flex md:col-span-4 sm:col-span-6 col-span-12 ml-5">
        <va-file-upload v-model="financialSubmissionFile" type="single">
          2. <span style="color: blue;text-decoration:underline;">Financial Submission</span>
        </va-file-upload>
      </div>
      <div class="flex md:col-span-4 sm:col-span-6 col-span-12 ml-5">
        <va-file-upload v-model="othersSubmissionFile" type="single">
          3. <span style="color: blue;text-decoration:underline;">Others Submission</span>
        </va-file-upload>
      </div>

    </div>
  </va-card>
  <br />
 
    <br>
    <br>
    <br>

      <span><va-button icon="chevron_left" type="submit" @click="onBack()" color="#36e9f6"><Loader v-if="loader" />Previous Page</va-button></span>
      <span style="float:right"><va-button icon="check" type="submit" @click="onCreate()" color="success"><Loader v-if="loader" />Save Record</va-button></span>
  
  

 
</template>

<script>
import swal from 'sweetalert2';
import Loader from "../../components/loader.vue";
import moment from 'moment';
export default {
  components: { Loader },
  data() {
    return {
      loader:false,
      clientList: [],
      date: "",
      client:"",
      price:0,
      userdetails:"",
      remark:"",
      refNo:"",
      title:"",


      tenderRequirementFile:'',
      technicalSubmissionFile:'',
      financialSubmissionFile:'',
      othersSubmissionFile:'',
      
    };
  },
  mounted() {
   

  },
  beforeMount() {
    
    this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
    this.GetClientList();
  
    
  },
  methods: {
   
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
                    const headers = {
                      Authorization: "Bearer " + this.userdetails.authorization.token,
                      Accept: "application/json",
                      "Content-Type": "application/json",
                    };
                    let body = new FormData();

                    body.append("title",this.title);
                    body.append("reference_no",this.refNo);
                    body.append("client_id",this.client);
                    body.append("submission_date",this.date);
                    body.append("submission_price",this.price );
                    body.append("tender",this.tenderRequirementFile);
                    body.append("technical",this.technicalSubmissionFile);
                    body.append("financial",this.financialSubmissionFile);
                    body.append("others", this.othersSubmissionFile);
                    body.append("remark", this.remark);
                    body.append("user_id",this.userdetails.user.user_id );

                          const response = await this.$axios.post(
                              "tenderStore", body ,{headers}
                          );
                          console.log("response", response.data);
                          if (response.data.code == 200) {
                            this.loader = false;
                              this.resetModel();
                              swal.fire('Record Successfully Saved', '', 'success')
                              this.onBack();
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
                  this.loader = false;
                      swal.fire('Record are not saved', '', 'info')
                  }
              })
    },
    resetModel(){
      this.title="";
      this.client="";
      this.refNo="";
      this.date="";
      this.price=0.00;
      this.remark="";

      this.tenderRequirementFile="";
      this.technicalSubmissionFile="";
      this.financialSubmissionFile="";
      this.othersSubmissionFile="";

    },
    //edit record
   onBack(){
    this.$router.push({ name: 'tender' });
   },
   getFormattedDate(date) {
            return moment(date).format("DD/MM/YYYY")
    },
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
    
  
     
   

  }
};

</script>

<style scoped></style>
