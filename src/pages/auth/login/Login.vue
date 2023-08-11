<template>
  <form @submit.prevent="onsubmit">
    <va-input id="input-email" class="mb-4" v-model="emailuser" label="Email"  
    :error="!!emailErrors.length"
    :error-messages="emailErrors"></va-input>

    <va-input id="input-password" class="mb-4" type="password" v-model="passworduser" label="Password"
    :error="!!passwordErrors.length"
    :error-messages="passwordErrors"
    ></va-input>

    <div class="auth-layout__options flex items-right justify-between">
      <router-link class="ml-1 va-link" :to="{ name: 'recover-password' }">Recover Password</router-link>
      <router-link class="ml-1 va-link" :to="{ name: 'first-time' }">First Time User</router-link>
    </div>

    <div class="flex justify-center mt-4">
      <Loader v-if="loader" />
      <va-button class="my-0" @click="onsubmit" v-if="!loader">Login</va-button>
    </div>
    
  </form>
</template>

<script>
import Loader from "../../../components/loader.vue";
export default {

  name: "login",

  data() {
    return {
     loader:false,
     emailuser:'',
     passworduser:'',

     emailErrors:'',
     passwordErrors:'',
    };
  },
  components: { Loader },
  methods: {
    async onsubmit() {
      try{
        if (!this.emailuser) {
          this.emailErrors = "Email is Required!";
        }
        if (!this.passworduser) {
          this.passwordErrors = "Password is Required!";
        }
        if (this.emailuser && this.passworduser) {
          this.loader = true;
          const response = await this.$axios.post("login", {
            email: this.emailuser,
            password: this.passworduser
          });
          this.userdetail = response.data;
          if (this.userdetail.code == 200) {
            localStorage.setItem("userdetails",JSON.stringify(this.userdetail));
              this.loader = false;
              if(this.passworduser == '123456'){
                this.$router.push({ name: 'reset-password' });
              }else{
                this.$router.push({ name: 'dashboard' });
              }
          }
        }
      }catch (e) {
        this.loader = false;
        this.emailErrors = "Email and Password does not matched";
        this.passwordErrors = "Email and Password does not matched";
      }
    },
  },
};
</script>

