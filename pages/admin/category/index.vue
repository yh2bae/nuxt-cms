<template>

  <div class="content-wrapper mb-5">
    <section class="content-header">
      <div class="container-fluid">
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-folder"></i> CATEGORIES</h3>
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
            <div class="input-group mb-3">
               <div class="col-md-6 col-lg-6">
                  <div class="input-group-prepend">
                    <nuxt-link :to="{name: 'admin-category-create'}" class="btn btn-info btn-sm" style="padding-top: 8px;"><i
                        class="fa fa-plus-circle"></i> Add New Category</nuxt-link>
                  </div>
                </div>
                <div class="col-md-3 col-lg-6">
                  <div class="input-group float-right" style="width: 300px;">
                    <input type="text" v-model="search" @keypress.enter="searchData" class="form-control float-right" placeholder="Search">
                    <div class="input-group-append">
                      <button @click="searchData" type="submit" class="btn btn-default search-button">
                        <i class="fas fa-search"></i>
                      </button>
                    </div>
                  </div>
                </div>

            </div>
          </div>

          <!-- table -->
          <b-table striped bordered hover :items="categories" :fields="fields" show-empty>
            <template v-slot:cell(index)="data">
                {{ data.index + 1 }} .
            </template>
            <template v-slot:cell(image)="data">
              <img class="img-fluid" width="50" :src="$axios.defaults.baseURL + 'public/upload/categories/' + data.item.image" />
            </template>
            <template v-slot:cell(actions)="row">
              <b-button :to="{name: 'admin-category-edit-id', params: {id: row.item.id}}" variant="warning" size="sm">
                 <i class="fas fa-edit"></i>
              </b-button>
              <b-button variant="danger" size="sm" @click="deleteCategory(row.item.id)">
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
        title: 'Categories - CMS',
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
            thClass: 'No',
            tdClass: 'No'

          },
          {
            label: 'Gambar',
            key: 'image',
            tdClass: 'text-center',
            thStyle: {
              backgroundColor: '#186399'
            },
          },
          {
            label: 'Nama Category',
            key: 'name',
            thStyle: {
              backgroundColor: '#186399'
            },
          },
          {
            label: 'Actions',
            key: 'actions',
            thClass: 'text-center',
            tdClass: 'text-center',
            thStyle: {
              backgroundColor: '#186399'
            },
          }
        ],

        //state search
        search: ''
      }
    },

    //watch query URL
    watchQuery: ["q", "page"],

    async asyncData({ $axios, query }) {

      //page
      let page = query.page ? parseInt(query.page) : ''

      //search
      let search = query.q ? query.q : ''

      //fetching categories
      const categories = await $axios.$get(`/api/admin/categories?q=${search}&page=${page}`)

      return {
        'categories': categories.data.data,
        'pagination': categories.data
      }
    },

    methods: {

      //change page pagination
      changePage(page) {
        this.$router.push({
          path: this.$route.path,
          query: {
            q: this.$route.query.q,
            page: page
          }
        });
      },

      //searchData
      searchData() {
        this.$router.push({
          path: this.$route.path,
          query: {
            q: this.search
          }
        });
      },

      //deleteCategory method  
      deleteCategory(id) {
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
            this.$axios.delete(`/api/admin/categories/${id}`)
              .then(() => {

                //feresh data
                this.$nuxt.refresh()

                //alert
                this.$swal.fire({
                  title: 'Successfully!',
                  text: "Data Deleted Successfully!",
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
.search-button {
    color: #fff;
    background-color: #186399;
    border-color: #186399
}

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