<template>
  <div class="content-wrapper">
    <section class="content-header">
      <div class="container-fluid">
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-clone"></i> Add New Menu</h3>
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
          <form @submit.prevent="storeMenu">
            <div v-if="validation.name" class="mb-2">
              <b-alert show dismissible variant="danger">
                <span style="font-weight:600">
                  <i class="fas fa-exclamation-triangle"></i> {{ validation.name[0] }}
                </span>
              </b-alert>
            </div>
            <div v-if="validation.url" class="mb-2">
              <b-alert show dismissible variant="danger">
                <span style="font-weight:600">
                  <i class="fas fa-exclamation-triangle"></i> {{ validation.url[0] }}
                </span>
              </b-alert>
            </div>
            <div class="form-group">
              <label>Name Menu</label>
              <input type="text" v-model="menu.name" placeholder="Enter name menu" :class="{ 'is-invalid': validation.name }"  class="form-control">
            </div>

            <div class="form-group">
              <label>URL Menu</label>
              <input type="text" v-model="menu.url" placeholder="Enter URL menu" :class="{ 'is-invalid': validation.url }" class="form-control">
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
        title: 'Add New Menu - CMS',
      }
    },

    data() {
        return {
            //state menu
            menu: {
                name: '',
                url: ''
            },
            //state validation
            validation: []
        }
    },

    methods: {
        //storeMenu method
        async storeMenu() {

            //sending data to server
            await this.$axios.post('/api/admin/menus', {

                //data
                name: this.menu.name,
                url: this.menu.url
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
                    name: 'admin-menu'
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