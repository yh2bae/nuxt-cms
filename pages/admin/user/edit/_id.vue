<template>
  <div class="content-wrapper">
    <section class="content-header">
      <div class="container-fluid">
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-users"></i> Edit User</h3>
          <div class="card-tools">
            <nuxt-link :to="{name: 'admin-menu'}" type="button" class="btn btn-tool" title="Back Datatable Menu">
              <i class="fas fa-caret-square-left"></i>
            </nuxt-link>
            <button type="button" class="btn btn-tool" data-card-widget="collapse" title="Collapse">
              <i class="fas fa-minus"></i>
            </button>
            <button type="button" class="btn btn-tool" data-card-widget="remove" title="Remove">
              <i class="fas fa-times"></i>
            </button>
          </div>
        </div>
        <div class="card-body">
          <form @submit.prevent="updateUser">

            <div v-if="validation.name" class="mb-2">
              <b-alert show dismissible variant="danger">
                <span style="font-weight:600">
                  <i class="fas fa-exclamation-triangle"></i> {{ validation.name[0] }}
                </span>
              </b-alert>
            </div>
            <div v-if="validation.email" class="mb-2">
              <b-alert show dismissible variant="danger">
                <span style="font-weight:600">
                  <i class="fas fa-exclamation-triangle"></i> {{ validation.email[0] }}
                </span>
              </b-alert>
            </div>

            <div class="form-group">
              <label>Name</label>
              <input type="text" v-model="user.name" placeholder="Enter Name" :class="{ 'is-invalid': validation.name }"  class="form-control">
            </div>

            <div class="form-group">
              <label>Email Address</label>
              <input type="email" v-model="user.email" placeholder="Enter email address" :class="{ 'is-invalid': validation.email }"  class="form-control">
            </div>

            <div class="form-group">
              <label>PASSWORD</label>
              <input type="password" v-model="user.password" placeholder="Enter Password" class="form-control">
            </div>

            <button class="btn btn-warning btn-reset float-right" type="reset"><i class="fa fa-redo"></i>
              Reset
            </button>
            <button class="btn btn-info mr-1 btn-submit float-right" type="submit"><i class="fa fa-paper-plane"></i>
              Save
            </button>

          </form>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
  export default {
    //layout
    layout: 'admin',

    //meta
    head() {
      return {
        title: 'Edit User - SantriKoding.com - Belajar Koding Bahasa Indonesia Terlengkap',
      }
    },

    data() {
      return {
        //state user
        user: {
          name: '',
          email: '',
          password: ''
        },
        //state validation
        validation: []
      }
    },

    mounted() {

      //fetching data user by ID
      this.$axios.get(`/api/admin/users/${this.$route.params.id}`)

        .then(response => {

          //assing response data to state "user.name"
          this.user.name = response.data.data.name

          //assing response data to state "user.email"
          this.user.email = response.data.data.email
        })
    },

    methods: {
      //updateUser method
      async updateUser() {

        //sending data to server
        await this.$axios.put(`/api/admin/users/${this.$route.params.id}`, {

            //data
            name: this.user.name,
            email: this.user.email,
            password: this.user.password
          })
          .then(() => {

            //sweet alert
            this.$swal.fire({
              title: 'Succefully!',
              text: "Data Saved Successfully!",
              icon: 'success',
              showConfirmButton: false,
              timer: 2000
            })

            //redirect, if success store data
            this.$router.push({
              name: 'admin-user'
            })

          })
          .catch(error => {

            //assign error to state "validation"
            this.validation = error.response.data
          })
      }
    }

  }
</script>

<style>

</style>