<template>
  <form @submit.prevent="onsubmit">
    <div class="flex justify-center mt-1">
        <p>For first-time users, please contact your administrator to request user access.</p>
    </div>
    <div class="flex justify-center mt-1">
        <p>Thank You.</p>
    </div>
    <div class="flex justify-center mt-10">
      <va-button class="my-0" @click="onsubmit">Go to the Login Page</va-button>
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
      this.$router.push({ name: 'login' });
    },
  },
};
</script>

