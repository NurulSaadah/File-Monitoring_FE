<template>
  <div class="auth-layout grid grid-cols-12 content-center" style="background-color:rgb(6, 8, 131);">
    <div class="flex col-span-12 justify-center">
      <router-link class="py-5 justify-center flex" to="/">
        <img style="margin: 0px;padding: 0px;" src="../../assets/images/logo-afms2.png" />
      </router-link>
    </div>

    <div class="flex justify-center col-span-12 p-1">
      <va-card class="auth-layout__card">
        <va-card-content>
          <va-tabs v-model="tabIndex" center>
            <template #tabs>
              <va-tab name="login">Login</va-tab>
              <va-tab name="signup">Create New Account</va-tab>
            </template>
          </va-tabs>

          <va-separator />

          <div class="p-3">
            <router-view />
          </div>
        </va-card-content>
      </va-card>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'AuthLayout',
    components: { },
    setup() {
    
    },
    data() {
      return {
        selectedTabIndex: 0,
      }
    },
    computed: {
      tabIndex: {
        set(tabName) {
          this.$router.push({ name: tabName })
        },
        get() {
          return this.$route.name
        },
      },
    },
  }
</script>

<style lang="scss">
  .auth-layout {
    min-height: 100vh;
    background-image: linear-gradient(to right, var(--va-background-primary), var(--va-white));

    &__card {
      width: 100%;
      max-width: 600px;
    }
  }
</style>
