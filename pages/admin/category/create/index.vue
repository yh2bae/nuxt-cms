<template>
  <div class="content-wrapper">
    <section class="content-header">
      <div class="container-fluid">
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-folder"></i> Add New Category</h3>
          <div class="card-tools">
            <nuxt-link :to="{name: 'admin-category'}" type="button" class="btn btn-tool" title="Back Datatable Category">
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
          <form @submit.prevent="storeCategory">
            <div v-if="validation.image" class="mb-2">
              <b-alert show dismissible variant="danger">
                <span style="font-weight:600">
                  <i class="fas fa-exclamation-triangle"></i> {{ validation.image[0] }}
                </span>
              </b-alert>
            </div>
            <div v-if="validation.name" class="mb-2">
              <b-alert show dismissible variant="danger">
                <span style="font-weight:600">
                  <i class="fas fa-exclamation-triangle"></i> {{ validation.name[0] }}
                </span>
              </b-alert>
            </div>
            <div class="form-group">
              <label>Image</label>
              <div id="preview">
                <img v-if="url" :src="url" class="mb-3 rounded" />
              </div>
              <div class="custom-file">
                  <input type="file" @change="handleFileChange"  :class="{ 'is-invalid': validation.image }" class="custom-file-input" >
                  <label class="custom-file-label" for="exampleInputFile">{{ImageFileName}}</label>
              </div>
            </div>
            

            <div class="form-group">
              <label>Name Category</label>
              <input type="text" v-model="category.name" :class="{ 'is-invalid': validation.name }"
                placeholder="Masukkan Nama Category" class="form-control">
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
        title: 'Add New Category - CMS',
      }
    },

    data() {
      return {
        //state category
        category: {
          image: '',
          name: ''
        },
        //state validation
        validation: [],
        url: null,
        ImageFileName: ''
      }
    },

    methods: {

      handleFileChange(e) {

        //get image
        const file = e.target.files[0];
        this.url = URL.createObjectURL(file);
        this.ImageFileName = e.target.files[0].name;

        let image = this.category.image = e.target.files[0];
        

        //check fileType
        if (!image.type.match('image.*')) {
          
          //if fileType not allowed, then clear value and set null
          e.target.value = ''

          this.category.image = null

          //show sweet alert
          this.$swal.fire({
            title: 'OOPS!',
            text: "File Format Not Supported!",
            icon: 'error',
            showConfirmButton: false,
            timer: 2000
          })
        }
        

        
        

      },

      //storeCategory method
      async storeCategory() {

        //define formData
        let formData = new FormData();

        formData.append('image', this.category.image)
        formData.append('name', this.category.name)

        //sending data to server
        await this.$axios.post('/api/admin/categories', formData)
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
              name: 'admin-category'
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
#preview {
  display: flex;
}

#preview img {
  max-width: 100px;
  max-height: 100px;
}
</style>