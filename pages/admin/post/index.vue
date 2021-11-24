<template>

  <div class="content-wrapper mb-5">
    <section class="content-header">
      <div class="container-fluid">
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-book-open"></i> POSTS</h3>
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
                    <nuxt-link :to="{name: 'admin-post-create'}" class="btn btn-info btn-sm" style="padding-top: 8px;"><i
                        class="fa fa-plus-circle"></i> Add New Post</nuxt-link>
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
          <b-table striped bordered hover :items="posts" :fields="fields" show-empty>
              <template v-slot:cell(index)="data">
                {{ data.index + 1 }} .
              </template>
              <template v-slot:cell(comments)="row">
                <i class="fa fa-comments"></i> {{ row.item.comments.length }}
              </template>
              <template v-slot:cell(actions)="row">
                <b-button :to="{name: 'admin-post-edit-id', params: {id: row.item.id}}" variant="success" size="sm">
                  <i class="fas fa-edit"></i>
                </b-button>
                <b-button variant="danger" size="sm" @click="deletePost(row.item.id)">
                  <i class="fas fa-trash"></i>
                </b-button>
              </template>
          </b-table>

           <!-- pagination -->
          <b-pagination v-model="pagination.current_page" :total-rows="pagination.total" :per-page="pagination.per_page"
            @change="changePage" align="right" class="mt-3">
          </b-pagination>

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
        title: 'Posts - CMS',
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

          },
          {
            label: 'Post Title',
            key: 'title',
            sortable: true,
            thStyle: { backgroundColor: '#186399'},
            
          },
          {
            label: 'Category',
            key: 'category.name',
            sortable: true,
            thStyle: { backgroundColor: '#186399'},
          },
          {
            label: 'Author',
            key: 'user.name',
            sortable: true,
            thStyle: { backgroundColor: '#186399'},
          },
          {
            label: 'Comments',
            key: 'comments',
            thStyle: { backgroundColor: '#186399'},
            thClass: 'text-center',
            tdClass: 'text-center',
          },
          {
            label: 'Actions',
            key: 'actions',
            thClass: 'text-center',
            tdClass: 'text-center',
            thStyle: { backgroundColor: '#186399'}
          }
        ],

        //state search
        search: ''
      }
    },

    //watch query URL
    watchQuery: ["q","page"],

    async asyncData({ $axios, query }) {

      //page
      let page = query.page ? parseInt(query.page) : ''

      //search
      let search = query.q ? query.q : ''

      //fetching posts
      const posts = await $axios.$get(`/api/admin/posts?q=${search}&page=${page}`)

      return {
        'posts': posts.data.data,
        'pagination': posts.data
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

      //deletePost method  
      deletePost(id) {
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
            this.$axios.delete(`/api/admin/posts/${id}`)
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