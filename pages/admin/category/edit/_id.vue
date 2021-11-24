<template>
  <div class="content-wrapper">
    <section class="content-header">
      <div class="container-fluid">
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-folder"></i> Edit Category</h3>
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
          <form @submit.prevent="updateCategory">
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
                  <input type="file"  @change="handleFileChange"  :class="{ 'is-invalid': validation.image }" class="custom-file-input" >
                  <label class="custom-file-label" for="exampleInputFile">{{ImageFileName}}</label>
              </div>
            </div>

            <div class="form-group">
              <label>Name Category</label>
              <input type="text" v-model="category.name" placeholder="Enter Name Category" class="form-control">
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
        title: 'Edit Category - CMS',
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

    mounted() {
      
      //fetching data category by ID
      this.$axios.get(`/api/admin/categories/${this.$route.params.id}`)
        
        .then(response => {
          
          //assing response data to state "category"
          this.category.name = response.data.data.name
          this.category.image = response.data.data.image
        })
    },

    methods: {

      handleFileChange(e) {

        //get image
        const file = e.target.files[0];
        this.url = URL.createObjectURL(file);
        this.ImageFileName = e.target.files[0].name;

        let image = this.category.image = e.target.files[0]

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

      //updateCategory method
       async updateCategory() {

         //define formData
        let formData = new FormData();

        formData.append('image', this.category.image)
        formData.append('name', this.category.name)
        formData.append("_method", "PATCH")

        //sending data to server
        await this.$axios.post(`/api/admin/categories/${this.$route.params.id}`, formData)
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