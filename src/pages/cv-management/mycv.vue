<template>
  <va-card class="col-span-12 sm:col-span-12" stripe stripe-color="info">
    <va-card-title>Curiculum Vitae: My CV</va-card-title>
    <va-card-content>
    <div class="mt-5">
      Academic Qualification(s)
    </div>
    <br>
    <table class="va-table va-table--striped va-table--hoverable w-full">
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
        <td>

        </td>
        <td>
          <va-input class="form-control qualification_level" placeholder="" v-model="qualification_level"/>
        </td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td @click="test">
          <span class="add-td"><i class="fa fa-plus"></i></span>
        </td>

      </tbody>
    </table>

    <br>
    <br>

    <div class="mt-5">
      Professional Body
    </div>
    <br>
    <table class="va-table va-table--striped va-table--hoverable w-full">
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
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td @click="test">
          <span class="add-td"><i class="fa fa-plus"></i></span>
        </td>

      </tbody>
    </table>

    <br>
    <br>
    <div class="mt-5">
      Professional Certificates
    </div>
    <br>
    <table class="va-table va-table--striped va-table--hoverable w-full">
      <thead>
          <tr>
            <th>No</th>
            <th>Project Title</th>
            <th>Sector</th>
            <th>Client Name</th>
            <th>Year of the Project</th>
            <th>Position in the Project</th>
            <th> </th>
          </tr>
        </thead>

      <tbody>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td @click="test">
          <span class="add-td"><i class="fa fa-plus"></i></span>
        </td>

      </tbody>
    </table>

    <br>
    <br>
    <div class="mt-5">
      Work Experience
    </div>
    <br>
    <table class="va-table va-table--striped va-table--hoverable w-full">
      <thead>
          <tr>
            <th>No</th>
            <th>Company Name</th>
            <th>Position</th>
            <th>Start Date</th>
            <th>End Date</th>
            <th>Year of Experience</th>
            <th> </th>
          </tr>
        </thead>

      <tbody>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td @click="test">
          <span class="add-td"><i class="fa fa-plus"></i></span>
        </td>

      </tbody>
    </table>
    <br>
    <br>
    <span style="float: right"><va-button icon="add_circle_outline" type="submit" @click="onSubmit()"><Loader v-if="loader" />Save Record</va-button></span>
    <span><va-button icon="chevron_left" type="submit" @click="onBack()" color="#36e9f6"><Loader v-if="loader" />Previous Page</va-button></span>

  </va-card-content>
    </va-card>
</template>

<script>
import swal from 'sweetalert2';
import Loader from "../../components/loader.vue";

export default {
  components: {Loader},
  data() {

    return {
      loader:false,
    };
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
  },

  methods: {

    test() {
    },

    async onSubmit() {
    },
  }
}
</script>

<style scoped></style>
