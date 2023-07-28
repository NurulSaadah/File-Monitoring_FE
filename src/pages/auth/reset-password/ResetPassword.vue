<template>
  <form @submit.prevent="onsubmit">
    <div class="flex justify-center mt-0 mb-3">
    <span style="color: red;">Please Reset Your Password !</span>
    </div>

    <va-input id="input-email" class="mb-4" v-model="emailuser" label="Email" disabled></va-input>

    <va-input id="input-passwor" class="mb-4" type="password" v-model="passworduser" label="New Password"
    :error="!!passwordErrors.length"
    :error-messages="passwordErrors"
    ></va-input>

    <va-input id="input-passwor" class="mb-4" type="password" v-model="confirmpassworduser" label="Confirm Password"
    :error="!!confirmpasswordErrors.length"
    :error-messages="confirmpasswordErrors"
    ></va-input>

    <div class="flex justify-center mt-4">
      <va-button class="my-0" @click="onsubmit">Reset Password</va-button>
    </div>
    
  </form>
</template>

<script>
import swal from 'sweetalert2';
export default {

  name: "login",
  beforeMount() {
    this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
    this.emailuser=this.userdetails.user.email
  },

  data() {
    return {
     emailuser:'',
     passworduser:'',
     confirmpassworduser:'',

     passwordErrors:'',
     confirmpasswordErrors:'',
    };
  },

  methods: {
    async onsubmit() {
      try{
        if (!this.passworduser) {
          this.passwordErrors = "Password is Required!";
        }
        if (!this.confirmpassworduser) {
          this.confirmpasswordErrors = "Password is Required!";
        }
        if (this.passworduser == this.confirmpassworduser) {
          const response = await this.$axios.post("reset", {
            password: this.passworduser,
            userId:this.userdetails.user.user_id,
          });
          if (response.data.code == 200) {
            swal.fire('Password Successfully Updated.Please Login to Continue', '', 'success')
            this.$router.push({ name: 'login' });
          }
        }else{
          this.passwordErrors = "Password does not matched";
          this.confirmpasswordErrors = "Password does not matched";
        }
      }catch (e) {
        this.passwordErrors = "Password does not matched";
        this.confirmpasswordErrors = "Password does not matched";
      }
    },
  },
};
</script>

