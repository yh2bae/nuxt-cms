<template>
  <div class="content-wrapper mb-5">
    <section class="content-header">
      <div class="container-fluid">
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-book-open"></i> Edit Post</h3>
          <div class="card-tools">
            <nuxt-link :to="{name: 'admin-post'}" type="button" class="btn btn-tool" title="Back Datatable Post">
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
          <form @submit.prevent="updatePost">
            <div v-if="validation.image" class="mb-2">
              <b-alert show dismissible variant="danger">
                <span style="font-weight:600">
                  <i class="fas fa-exclamation-triangle"></i> {{ validation.image[0] }}
                </span>
              </b-alert>
            </div>
            <div v-if="validation.title" class="mb-2">
              <b-alert show dismissible variant="danger">
                <span style="font-weight:600">
                  <i class="fas fa-exclamation-triangle"></i> {{ validation.title[0] }}
                </span>
              </b-alert>
            </div>
            <div v-if="validation.category_id" class="mb-2">
              <b-alert show dismissible variant="danger">
                <span style="font-weight:600">
                  <i class="fas fa-exclamation-triangle"></i> {{ validation.category_id[0] }}
                </span>
              </b-alert>
            </div>
            <div v-if="validation.content" class="mb-2">
              <b-alert show dismissible variant="danger">
                <span style="font-weight:600">
                  <i class="fas fa-exclamation-triangle"></i> {{ validation.content[0] }}
                </span>
              </b-alert>
            </div>
            <div v-if="validation.description" class="mb-2">
              <b-alert show dismissible variant="danger">
                <span style="font-weight:600">
                  <i class="fas fa-exclamation-triangle"></i> {{ validation.description[0] }}
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
              <label>JUDUL POST</label>
              <input type="text" v-model="post.title" placeholder="Masukkan Judul Post" class="form-control">
              <div v-if="validation.title" class="mt-2">
                <b-alert show variant="danger">{{ validation.title[0] }}</b-alert>
              </div>
            </div>

            <div class="form-group">
              <label>CATEGORY</label>
              <multiselect v-model="post.category_id" :options="categories" label="name" track-by="id"
                :searchable="true"></multiselect>
              <div v-if="validation.category_id" class="mt-2">
                <b-alert show variant="danger">{{ validation.category_id[0] }}</b-alert>
              </div>
            </div>

            <div class="form-group">
              <label>KONTEN POST</label>
              <client-only placeholder="loading...">
                <ckeditor-nuxt v-model="post.content" :config="editorConfig" />
              </client-only>
              <div v-if="validation.content" class="mt-2">
                <b-alert show variant="danger">{{ validation.content[0] }}</b-alert>
              </div>
            </div>

            <div class="form-group">
              <label>TAGS</label>
              <multiselect v-model="post.tags" :options="tags" label="name" track-by="id" :multiple="true"
                :searchable="true">
              </multiselect>
            </div>

            <div class="form-group">
              <label>DESCRIPTION</label>
              <textarea v-model="post.description" class="form-control" rows="3"
                placeholder="Masukkan Deskripsi Singkat"></textarea>
              <div v-if="validation.description" class="mt-2">
                <b-alert show variant="danger">{{ validation.description[0] }}</b-alert>
              </div>
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
        title: 'Edit Post - CMS',
      }
    },

    components: {
      'ckeditor-nuxt': () => {
        if (process.client) {
          return import('@blowstack/ckeditor-nuxt')
        }
      },
    },

    data() {
      return {
        //state post
        post: {
          image: '',
          title: '',
          category_id: '',
          content: '',
          description: '',
          tags: []
        },

        //state categories
        categories: [],

        //state tags
        tags: [],

        //state validation
        validation: [],
        url: null,
        ImageFileName: '',

        //config CKEDITOR
        editorConfig: {
          removePlugins: ['Title'],
          simpleUpload: {
            uploadUrl: 'https://cms-api.appdev.my.id/api/web/posts/storeImage'
          }
        }
      }
    },

    mounted() {

      //fetching data post
      this.$axios.get(`/api/admin/posts/${this.$route.params.id}`)

        .then(response => {

          //assing response data to state "post.title"
          this.post.title = response.data.data.title

          //assing response data to state "post.category_id"
          this.post.category_id = response.data.data.category

          //assing response data to state "post.content"
          this.post.content = response.data.data.content

          //assing response data to state "post.tags"
          this.post.tags = response.data.data.tags

          //assing response data to state "post.description"
          this.post.description = response.data.data.description
        })


      //fetching data categories
      this.$axios.get('/api/admin/categories')

        .then(response => {

          //assing response data to state "categories"
          this.categories = response.data.data.data
        })

      //fetching data tags
      this.$axios.get('/api/admin/tags')

        .then(response => {

          //assing response data to state "tags"
          this.tags = response.data.data.data
        })

    },

    methods: {

      handleFileChange(e) {

        //get image
        const file = e.target.files[0];
        this.url = URL.createObjectURL(file);
        this.ImageFileName = e.target.files[0].name;

        let image = this.post.image = e.target.files[0]

        //check fileType
        if (!image.type.match('image.*')) {

          //if fileType not allowed, then clear value and set null
          e.target.value = ''

          this.post.image = null

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

      async updatePost() {

        /**
         * get selected Tags
         */
        let tags = this.post.tags
        let selectedTags = []

        tags.forEach((tag) => {
          selectedTags.push(tag.id)
        })

        //define formData
        let formData = new FormData();

        formData.append('image', this.post.image)
        formData.append('title', this.post.title)
        formData.append('category_id', this.post.category_id ? this.post.category_id.id : '')
        formData.append('content', this.post.content)
        formData.append('description', this.post.description)
        formData.append("_method", "PATCH")

        //append tags array
        for (var i = 0; i < selectedTags.length; i++) {
            formData.append('tags[]', selectedTags[i]);
        }

        //sending data to server
        await this.$axios.post(`/api/admin/posts/${this.$route.params.id}`, formData)
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
              name: 'admin-post'
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
  .ck-editor__editable {
    min-height: 200px;
  }
</style>