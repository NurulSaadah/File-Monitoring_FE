<template>
  <va-card class="col-span-12 sm:col-span-12" stripe stripe-color="info">
    <va-card-title>Curiculum Vitae: My CV</va-card-title>
    <va-card-content>
      <div class="mt-5">Academic Qualification(s)</div>
      <br />
      <table class="va-table va-table--striped va-table--hoverable w-full" id="qualificationlevel">
        <thead>
          <tr>
            <th>No</th>
            <th>Qualification Level</th>
            <th>Field of Study</th>
            <th>Specialization</th>
            <th>Year Awarded</th>
            <th>Institution Name</th>
            <th></th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(qualification,idx) in qualificationLevel" :key="idx">
          <td>
            {{ idx + 1 }}
          </td>
          <td>
            {{ qualification.qualification_level }}
          </td>
          <td>
            {{ qualification.field_of_study }}
          </td>
          <td>
            {{ qualification.specialization }}
          </td>
          <td>
            {{ qualification.year_awarded }}
          </td>
          <td>
            {{ qualification.institution_name }}
          </td>

          </tr>
        </tbody>
      </table>

      <br />
      <br />

      <div class="mt-5">Professional Body</div>
      <br />
      <table class="va-table va-table--striped va-table--hoverable w-full" id="professionalbody">
        <thead>
          <tr>
            <th>No</th>
            <th>Name of Professional Body</th>
            <th>Membership Number</th>
            <th>Membership Expiry Date</th>
            <th></th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(professionalbody,idx) in professionalBody" :key="idx">
          <td>
            {{ idx + 1 }}
          </td>
          <td>
            {{ professionalbody.name }}
          </td>
          <td>
            {{ professionalbody.membership_no }}
          </td>
          <td>
            {{ professionalbody.membership_expiry_date }}
          </td>
          </tr>
        </tbody>
      </table>

      <br />
      <br />
      <div class="mt-5">Professional Certificate(s)</div>
      <br />
      <table class="va-table va-table--striped va-table--hoverable w-full" id="projectlist">
        <thead>
          <tr>
            <th>No</th>
            <th>Certificate Name</th>
            <th>Certificates</th>
            <th>Date</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(procert,idx) in procertList" :key="idx">
          <td>
            {{ idx + 1 }}
          </td>
          <td>
            {{ procert.certificate_name }}
          </td>
          <td>
            {{ procert.certificate }}
          </td>
          <td>
            {{ procert.date }}
          </td>
          </tr>
        </tbody>
      </table>


      <br />
      <br />
      <div class="mt-5">List Of Projects</div>
      <br />
      <table class="va-table va-table--striped va-table--hoverable w-full" id="projectlist">
        <thead>
          <tr>
            <th>No</th>
            <th>Project Title</th>
            <th>Sector</th>
            <th>Client Name</th>
            <th>Year of the Project</th>
            <th>Position in the Project</th>
            <th></th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(project,idx) in projectList" :key="idx">
          <td>
            {{ idx + 1 }}
          </td>
          <td>
            {{ project.project_title }}
          </td>
          <td>
            {{ project.government_private }}
          </td>
          <td>
            {{ project.client_id }}
          </td>
          <td>
            {{ project.project_year }}
          </td>
          <td>
            {{ project.position_in_project }}
          </td>
          </tr>
        </tbody>
      </table>
      <br />
      <br />
      <div class="mt-5">Work Experience</div>
      <br />
      <table class="va-table va-table--striped va-table--hoverable w-full" id="workexperience">
        <thead>
          <tr>
            <th>No</th>
            <th>Company Name</th>
            <th>Position</th>
            <th>Start Date</th>
            <th>End Date</th>
            <th>Year of Experience</th>
            <th></th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(workexp,idx) in workexpList" :key="idx">
          <td>
            {{ idx + 1 }}
          </td>
          <td>
            {{ workexp.company_name }}
          </td>
          <td>
            {{ workexp.position }}
          </td>
          <td>
            {{ workexp.start_date }}
          </td>
          <td>
            {{ workexp.end_date }}
          </td>
          <td>
            {{ workexp.year_of_experience }}
          </td>
          </tr>
        </tbody>
      </table>
      <br />
      <br />
    </va-card-content>
  </va-card>
</template>

<script>
  import swal from 'sweetalert2'
  import Loader from '../../components/loader.vue'

  export default {
    components: { Loader },
    data() {
      return {
        loader: false,
        qualificationLevel: [],
        professionalBody: [],
        procertList:[],
        projectList:[],
        workexpList:[],
      }
    },

    beforeMount() {
      // var num=1;
      // $(".add-td").click(function (i) {
      //               num = num + 1;
      //               console.log(num);
      //               $(".block:last").after(
      //                   '<tr class="block"> <td>' +
      //                   parseInt(num) +
      //                   '</td> <td> <input type="text" class="form-control job" name=""> </td></tr>'
      //               );
      //           });
      this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
      this.GetQualificationLevel();
      this.GetProfessionalBody();
      this.GetProfessionalCert();
      this.GetProjectList();
      this.GetWorkExperience();
    },

    methods: {
      async GetQualificationLevel() {
        const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "getQualificationLevelList",
        {headers}
      );

      if(response.data.code == 200 || response.data.code == '200'){
        this.qualificationLevel = response.data.list;
      }

      },

      async GetProfessionalBody() {
        const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "getProfessionalBody",
        {headers}
      );

      if(response.data.code == 200 || response.data.code == '200'){
        this.professionalBody = response.data.list;
      }

      },

      async GetProfessionalCert() {
        const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "getProfessionalCert",
        {headers}
      );

      if(response.data.code == 200 || response.data.code == '200'){
        this.procertList = response.data.list;
      }

      },

      async GetProjectList() {
        const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "getProjectList",
        {headers}
      );

      if(response.data.code == 200 || response.data.code == '200'){
        this.projectList = response.data.list;
      }

      },

      async GetWorkExperience() {
        const headers = {
        Authorization: "Bearer " + this.userdetails.authorization.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "getWorkExperience",
        {headers}
      );

      if(response.data.code == 200 || response.data.code == '200'){
        this.workexpList = response.data.list;
      }

      },
      test() {},

      async onSubmit() {},
    },
  }
</script>

<style scoped></style>
