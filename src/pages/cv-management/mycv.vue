<template>
  <va-card class="col-span-12 sm:col-span-12" style="width: 1500px;" stripe stripe-color="info">
    <va-card-title>Curiculum Vitae: My CV</va-card-title>
    <va-card-content>
      <div class="mt-5">Academic Qualification(s)</div>
      <br />
      <table class="va-table w-full" id="qualificationlevel">
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
        <tbody class="optionBox">
          <tr class="plusbtn">
            <td>1</td>
            <td>
              <va-input type="test" class="form-control quals" name="" placeholder="Enter Qualification Level"/>
            </td>
            <td>
              <va-input class="form-control study" name="" placeholder="Enter Field of Study"/>
            </td>
            <td>
              <va-input class="form-control special" name="" placeholder="Enter Specialization" />
            </td>
            <td>
              <va-input class="form-control yearawarded" name="" placeholder="Enter Year Awarded" />
            </td>
            <td>
              <va-input class="form-control institution" name="" placeholder="Enter Institution Name" />
            </td>
            <td>
              <!-- <va-button @click="addField()" class="add-button">Add Field</va-button> -->
              <!-- <span class="add-td"><i class="fa fa-plus" @click="add(index)" v-show="index == quals.length-1"></i>
              <i class="fa fa-minus" @click="remove(index)" v-show="index && quals.length+1"></i></span> -->
              <span class="add-td"><i class="fa fa-plus"></i></span>
            </td>
          </tr>
        </tbody>
      </table>

      <br />
      <br />

      <div class="mt-5">Professional Body</div>
      <br />
      <table class="va-table">
        <thead>
          <tr>
            <th>No</th>
            <th>Name of Professional Body</th>
            <th>Membership Number</th>
            <th>Membership Expiry Date</th>
            <th></th>
          </tr>
        </thead>

        <tbody class="optionBox">
          <tr class="plusbtn2">
          <td>1</td>
          <td>
            <va-input class="form-control professionalbody" name="" placeholder="Enter Name of Professional Body"/>
          </td>
          <td>
            <va-input class="form-control membershipnumber" name="" placeholder="Enter Membership Number"/>
          </td>
          <td>
            <va-input class="form-control expirydate" name="" placeholder="Enter Membership Expiry Date"/>
          </td>
          <td>
            <span class="add-td2"><i class="fa fa-plus"></i></span>
          </td>
          </tr>
        </tbody>
      </table>

      <br />
      <br />

      <div class="mt-5">Professional Certificate(s)</div>
      <br />
      <table class="va-table">
        <thead>
          <tr>
            <th>No</th>
            <th>Certificate Name</th>
            <th>Certificate</th>
            <th>Date</th>
            <th></th>
          </tr>
        </thead>

        <tbody class="optionBox">
          <tr class="plusbtn3">
          <td>1</td>
          <td>
            <va-input class="form-control professionalbody" name="" placeholder="Enter Name of Professional Body"/>
          </td>
          <td>
            <va-input class="form-control membershipnumber" name="" placeholder="Enter Membership Number"/>
          </td>
          <td>
            <va-input
              type="date"
              v-model="date_from"
              placeholder="dd/mm/yyyy"
              label="FROM"
              id="dateFrom"
              @change="filterRows()"
            />
          </td>
          <td>
            <span class="add-td3"><i class="fa fa-plus"></i></span>
          </td>
          </tr>
        </tbody>
      </table>


      <br />
      <br />
      <div class="mt-5">List Of Projects</div>
      <br />
      <table class="va-table va-table--striped va-table--hoverable w-full">
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
          <td>1</td>
          <td>
            <va-input class="form-control" placeholder="Enter Project Title" v-model="project_title" />
          </td>
          <td>
            <va-input class="form-control" placeholder="Enter Sector" v-model="sector" />
          </td>
          <td>
            <va-input class="form-control" placeholder="Enter Client Name" v-model="client_name" />
          </td>
          <td>
            <va-input class="form-control" placeholder="Enter Year of the Project" v-model="year_project" />
          </td>
          <td>
            <va-input class="form-control" placeholder="Enter Position in the Project" v-model="position_project" />
          </td>
          <td @click="test">
            <span class="add-td"><i class="fa fa-plus"></i></span>
          </td>
        </tbody>
      </table>

      <br />
      <br />
      <div class="mt-5">Work Experience</div>
      <br />
      <table class="va-table va-table--striped va-table--hoverable w-full">
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
          <td></td>
          <td>
            <va-input class="form-control" placeholder="Enter Company Name" v-model="company_name" />
          </td>
          <td>
            <va-input class="form-control" placeholder="Enter Position" v-model="experience_position" />
          </td>
          <td>
            <va-input
              type="date"
              v-model="date_from"
              placeholder="dd/mm/yyyy"
              label="FROM"
              id="dateFrom"
              @change="filterRows()"
            />
          </td>
          <td>
            <va-input
              type="date"
              v-model="date_to"
              placeholder="dd/mm/yyyy"
              label="TO"
              id="dateTo"
              @change="filterRows()"
            />
          </td>
          <td>
            <va-input class="form-control" placeholder="Enter Year of Experience" v-model="year_experience" />
          </td>
          <td @click="test">
            <span class="add-td"><i class="fa fa-plus"></i></span>
          </td>
        </tbody>
      </table>
      <br />
      <br />
      <div class="ml-auto" :class="SidebarAccess != 1 ? 'hide' : ''" v-if="!pid">
        <span style="float: right"
          ><va-button icon="add_circle_outline" type="submit" @click="onSubmit()"
            ><Loader v-if="loader" />Save Record</va-button
          ></span
        >
        <span
          ><va-button icon="chevron_left" type="submit" @click="onBack()" color="#36e9f6"
            ><Loader v-if="loader" />Previous Page</va-button
          ></span
        >
      </div>
    </va-card-content>
  </va-card>
</template>

<script>
  import swal from 'sweetalert2'
  import Loader from '../../components/loader.vue'
  import $ from 'jquery'

  export default {
    components: { Loader },
    data() {
      return {
        loader: false,
        qualificationLevel: [],
        qualification: [],
        pid: 0,
        quals: [''],
      }
    },

    beforeMount() {
      this.userdetails = JSON.parse(localStorage.getItem('userdetails'))
      $(document).ready(function () {
        var num = 1
        $('.add-td').click(function (i) {
          num = num + 1
          console.log(num)
          $('.plusbtn:last').after(
            '<tr class="plusbtn"> <td>' +
              parseInt(num) +
              '</td> <td>' +
              '<div class="va-input-wrapper va-input-wrapper--solid va-input form-control qual" style="--va-input-wrapper-color: #154EC1; --va-0-background-computed: #ECF0F1; --va-1-text-color-computed: #262824;"><div class="va-input-wrapper__container">'+
              '<div class="va-input-wrapper__field">'+
              '<div class="va-input-wrapper__text">'+
              '<input class="form-control qual va-input_content_input" name="" placeholder="Enter Qualification Level"/></div></div></div>' +
              '</td> <td>'+
              '<div class="va-input-wrapper va-input-wrapper--solid va-input form-control qual" style="--va-input-wrapper-color: #154EC1; --va-0-background-computed: #ECF0F1; --va-1-text-color-computed: #262824;"><div class="va-input-wrapper__container">'+
              '<div class="va-input-wrapper__field">'+
              '<div class="va-input-wrapper__text">'+
              '<input  class="form-control study va-input_content_input" name="" placeholder="Enter Field of Study"//></div></div></div>' +
              '</td> <td>'+
              '<div class="va-input-wrapper va-input-wrapper--solid va-input form-control qual" style="--va-input-wrapper-color: #154EC1; --va-0-background-computed: #ECF0F1; --va-1-text-color-computed: #262824;"><div class="va-input-wrapper__container">'+
              '<div class="va-input-wrapper__field">'+
              '<div class="va-input-wrapper__text">'+
              '<input class="form-control special va-input_content_input" name="" placeholder="Enter Specialization"/></div></div></div>' +
              '</td> <td>'+
              '<div class="va-input-wrapper va-input-wrapper--solid va-input form-control qual" style="--va-input-wrapper-color: #154EC1; --va-0-background-computed: #ECF0F1; --va-1-text-color-computed: #262824;"><div class="va-input-wrapper__container">'+
              '<div class="va-input-wrapper__field">'+
              '<div class="va-input-wrapper__text">'+
              '<input <va-input class="form-control yearawarded va-input_content_input" name="" placeholder="Enter Year Awarded"/></div></div></div>' +
              '</td> <td>'+
              '<div class="va-input-wrapper va-input-wrapper--solid va-input form-control qual" style="--va-input-wrapper-color: #154EC1; --va-0-background-computed: #ECF0F1; --va-1-text-color-computed: #262824;"><div class="va-input-wrapper__container">'+
              '<div class="va-input-wrapper__field">'+
              '<div class="va-input-wrapper__text">'+
              '<input class="form-control institution va-input_content_input" name="" placeholder="Enter Institution Name"/></div></div></div>' +
              '</td> <td>'+
              '<span class="remove"><i class="fa fa-times"></i></span></td></tr>'
          )
        })

        $('.add-td2').click(function (i) {
          num = num + 1
          console.log(num)
          $('.plusbtn2:last').after(
            '<tr class="plusbtn2"> <td>' +
              parseInt(num) +
              '</td> <td>' +
              '<div class="va-input-wrapper va-input-wrapper--solid va-input form-control professionalbody" style="--va-input-wrapper-color: #154EC1; --va-0-background-computed: #ECF0F1; --va-1-text-color-computed: #262824;"><div class="va-input-wrapper__container">'+
              '<div class="va-input-wrapper__field">'+
              '<div class="va-input-wrapper__text">'+
              '<input class="form-control professionalbody va-input_content_input" name="" placeholder="Enter Name of Professional Body"/></div></div></div>' +
              '</td> <td>'+
              '<div class="va-input-wrapper va-input-wrapper--solid va-input form-control qual" style="--va-input-wrapper-color: #154EC1; --va-0-background-computed: #ECF0F1; --va-1-text-color-computed: #262824;"><div class="va-input-wrapper__container">'+
              '<div class="va-input-wrapper__field">'+
              '<div class="va-input-wrapper__text">'+
              '<input class="form-control membershipnumber va-input_content_input" name="" placeholder="Enter Membership Number"/></div></div></div>' +
              '</td> <td>'+
              '<div class="va-input-wrapper va-input-wrapper--solid va-input form-control qual" style="--va-input-wrapper-color: #154EC1; --va-0-background-computed: #ECF0F1; --va-1-text-color-computed: #262824;"><div class="va-input-wrapper__container">'+
              '<div class="va-input-wrapper__field">'+
              '<div class="va-input-wrapper__text">'+
              '<input class="form-control expirydate va-input_content_input" name="" placeholder="Enter Membership Expiry Date"/></div></div></div>' +
              '</td> <td>'+
              '<span class="remove"><i class="fa fa-times"></i></span></td></tr>',
          )
        })

        $('.optionBox').on('click', '.remove', function () {
          num = num - 1
          $(this).closest('.plusbtn').remove()
          $(this).closest('.plusbtn2').remove()
          $(this).closest('.plusbtn2').remove()
        })
      })
    },

    methods: {
      add() {
        this.quals.push({
          qualification: '',
        })
        console.log(this.quals)
      },

      remove(index) {
        this.quals.splice(index, 1)
      },

      test() {},

      async onSubmit() {},
    },
  }
</script>

<style scoped></style>
