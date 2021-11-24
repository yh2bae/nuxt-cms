<template>
  <div class="content-wrapper">
    <section class="content-header">
      <div class="container-fluid">
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-tags"></i> Edit Tag</h3>
          <div class="card-tools">
            <nuxt-link :to="{name: 'admin-tag'}" type="button" class="btn btn-tool" title="Back Datatable Tags">
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
          <form @submit.prevent="updateTag">

            <div class="form-group">
              <div v-if="validation.name" class="mb-2">
                <b-alert show dismissible variant="danger">
                  <span style="font-weight:600">
                    <i class="fas fa-exclamation-triangle"></i> {{ validation.name[0] }}
                  </span>
                </b-alert>
              </div>
              <label>Name Tag</label>
              <input type="text" v-model="tag" placeholder="Enter Name Tag" class="form-control">
            </div>

            <button class="btn btn-info mr-1 btn-submit" type="submit"><i class="fa fa-paper-plane"></i>
              UPDATE</button>
            <button class="btn btn-warning btn-reset" type="reset"><i class="fa fa-redo"></i>
              RESET</button>

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
        title: 'Edit Tag - CMS',
      }
    },

    data() {
      return {
        //state tag
        tag: '',
        //state validation
        validation: []
      }
    },

    mounted() {
      
      //fetching data tag by ID
      this.$axios.get(`/api/admin/tags/${this.$route.params.id}`)
        
        .then(response => {
          
          //assing response data to state "tag"
          this.tag = response.data.data.name
        })
    },

    methods: {
      //updateTag method
       async updateTag() {

        //sending data to server
        await this.$axios.put(`/api/admin/tags/${this.$route.params.id}`, {

            //data
            name: this.tag
          })
          .then(() => {

            //sweet alert
            this.$swal.fire({
              title: 'Successfully!',
              text: "Data Updated Successfully!",
              icon: 'success',
              showConfirmButton: false,
              timer: 2000
            })

            //redirect, if success update data
            this.$router.push({
              name: 'admin-tag'
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