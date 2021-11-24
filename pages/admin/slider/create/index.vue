<template>
  <div class="content-wrapper">
    <section class="content-header">
      <div class="container-fluid">
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-tags"></i> Add New Slider</h3>
          <div class="card-tools">
            <nuxt-link :to="{name: 'admin-slider'}" type="button" class="btn btn-tool" title="Back Datatable Sliders">
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
          <form @submit.prevent="storeSlider">
            <div v-if="validation.image" class="mb-2">
              <b-alert show dismissible variant="danger">
                <span style="font-weight:600">
                  <i class="fas fa-exclamation-triangle"></i> {{ validation.image[0] }}
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
        title: 'Add New Slider - CMS',
      }
    },

    data() {
      return {
        //state slider
        slider: {
          image: '',
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

        let image = this.slider.image = e.target.files[0]

        //check fileType
        if (!image.type.match('image.*')) {

          //if fileType not allowed, then clear value and set null
          e.target.value = ''

          this.slider.image = null

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

      //storeSlider method
      async storeSlider() {

        //define formData
        let formData = new FormData();

        formData.append('image', this.slider.image)

        //sending data to server
        await this.$axios.post('/api/admin/sliders', formData)
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
              name: 'admin-slider'
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
  justify-content: center;
}

#preview img {
  max-width: 500px;
  max-height: 500px;
}
</style>