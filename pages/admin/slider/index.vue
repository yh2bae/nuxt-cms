<template>

  <div class="content-wrapper mb-5">
    <section class="content-header">
      <div class="container-fluid">
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-image"></i> SLIDERS</h3>
          <div class="card-tools">
            <button type="button" class="btn btn-tool" data-card-widget="collapse" title="Collapse">
              <i class="fas fa-minus"></i>
            </button>
            <button type="button" class="btn btn-tool" data-card-widget="remove" title="Remove">
              <i class="fas fa-times"></i>
            </button>
          </div>
        </div>
        <div class="card-body">
          <div class="form-group">
            <nuxt-link :to="{name: 'admin-slider-create'}" class="btn btn-info btn-sm p-2"><i
                class="fa fa-plus-circle"></i> Add New Slider</nuxt-link>
          </div>

          <!-- table -->
          <b-table striped bordered hover :items="sliders" :fields="fields" show-empty>
            <template v-slot:cell(index)="data">
                {{ data.index + 1 }} .
            </template>
            <template v-slot:cell(image)="data">
              <img class="img-fluid img-thumbnail" width="400" :src="$axios.defaults.baseURL + 'public/upload/sliders/' + data.item.image" />
            </template>
            <template v-slot:cell(actions)="row">
              <b-button variant="danger" size="sm" @click="deleteSlider(row.item.id)">
                  <i class="fas fa-trash"></i>
              </b-button>
            </template>
          </b-table>

          <!-- pagination -->
          <b-pagination v-model="pagination.current_page" :total-rows="pagination.total" :per-page="pagination.per_page"
            @change="changePage" align="right" class="mt-3"></b-pagination>

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
        title: 'Sliders - CMS',
      }
    },

    //data function
    data() {
      return {

        //table header
        fields: [{
            label: 'No.',
            key: 'index',
            thStyle: {
              backgroundColor: '#186399'
            },
            tdClass: 'No',
            thClass: 'No'

          },{
            label: 'Image',
            key: 'image',
            tdClass: 'text-center',
            thStyle: { backgroundColor: '#186399'},
          },
          {
            label: 'Actions',
            key: 'actions',
            thClass: 'text-center',
            tdClass: 'text-center',
            thStyle: { backgroundColor: '#186399'},
          }
        ],
      }
    },

    //watch query URL
    watchQuery: [ "page"],

    async asyncData({ $axios, query }) {

      //page
      let page = query.page ? parseInt(query.page) : ''  

      //fetching sliders
      const sliders = await $axios.$get(`/api/admin/sliders?page${page}`)

      return {
        'sliders': sliders.data.data,
        'pagination': sliders.data
      }
    },

    methods: {

      //change page pagination
      changePage(page) {
        this.$router.push({
          path: this.$route.path,
          query: {
            page: page
          }
        });
      },

      //deleteSlider method  
      deleteSlider(id) {
        this.$swal.fire({
          title: 'Are you sure?',
          text: "Want to delete this data!",
          icon: 'warning',
          showCancelButton: true,
          confirmButtonColor: '#d33',
          cancelButtonColor: '#3085d6',
          confirmButtonText: 'Yes, Delete!',
          cancelButtonText: 'No',
        }).then((result) => {
          if (result.isConfirmed) {

            //delete tag from server
            this.$axios.delete(`/api/admin/sliders/${id}`)
              .then(() => {

                //feresh data
                this.$nuxt.refresh()

                //alert
                this.$swal.fire({
                  title: 'BERHASIL!',
                  text: "Data Berhasil Dihapus!",
                  icon: 'success',
                  showConfirmButton: false,
                  timer: 2000
                })

              })
          }
        })
      }
    }

  }
</script>

<style>
.table th {
    color: #fff;
}

.No {
    width: 5%;
    text-align: center;
}

.page-item.active .page-link {
    z-index: 3;
    color: #fff;
    background-color: #186399;
    border-color: #186399;
}
</style>