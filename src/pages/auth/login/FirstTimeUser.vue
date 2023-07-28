<template>
  <form @submit.prevent="onsubmit">
    <va-input id="input-email" class="mb-4" v-model="emailuser" label="Email"  
    :error="!!emailErrors.length"
    :error-messages="emailErrors"></va-input>

    <va-input id="input-passwor" class="mb-4" type="password" v-model="passworduser" label="Password"
    :error="!!passwordErrors.length"
    :error-messages="passwordErrors"
    ></va-input>

    <div class="auth-layout__options flex items-right justify-between">
      <router-link class="ml-1 va-link" :to="{ name: 'recover-password' }">Recover Password</router-link>
      <router-link class="ml-1 va-link" :to="{ name: 'first-time' }">First Time User</router-link>
    </div>

    <div class="flex justify-center mt-4">
      <va-button class="my-0" @click="onsubmit">Login</va-button>
    </div>
    
  </form>
</template>

<script>
export default {

  name: "login",

  data() {
    return {
     emailuser:'',
     passworduser:'',

     emailErrors:'',
     passwordErrors:'',
    };
  },

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
          const response = await this.$axios.post("login", {
            email: this.emailuser,
            password: this.passworduser
          });
          this.userdetail = response.data;
          if (this.userdetail.code == 200) {
            localStorage.setItem("userdetails",JSON.stringify(this.userdetail));

              if(this.passworduser == '123456'){
                this.$router.push({ name: 'reset-password' });
              }else{
                this.$router.push({ name: 'dashboard' });
              }
          }
        }
      }catch (e) {
        this.emailErrors = "Email and Password does not matched";
        this.passwordErrors = "Email and Password does not matched";
      }
    },
  },
};
</script>

