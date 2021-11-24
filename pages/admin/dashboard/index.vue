<template>
  <!-- Content Wrapper. Contains page content -->
  <div class="content-wrapper">
    <section class="content-header">
      <div class="container-fluid">
      </div>
    </section>

    <section class="content">
      <div class="row">
        <div class="col-md-3 col-sm-6 col-12">
          <div class="small-box bg-info">
            <div class="inner">
              <h3>{{ users }}</h3>
              <p>User</p>
            </div>
            <div class="icon">
              <i class="fa fa-users"></i>
            </div>
            <a href="#" class="small-box-footer">Show <i class="fas fa-arrow-circle-right"></i></a>
          </div>
        </div>
        <div class="col-md-3 col-sm-6 col-12">
          <div class="small-box bg-success">
            <div class="inner">
              <h3>{{ categories }}</h3>
              <p>Categories</p>
            </div>
            <div class="icon">
              <i class="fa fa-folder"></i>
            </div>
            <nuxt-link :to="{name: 'admin-category'}" class="small-box-footer">Show <i class="fas fa-arrow-circle-right"></i></nuxt-link>
          </div>
        </div>
        <div class="col-md-3 col-sm-6 col-12">
          <div class="small-box bg-primary">
            <div class="inner">
              <h3>{{ posts }}</h3>
              <p>Posts</p>
            </div>
            <div class="icon">
              <i class="fa fa-book-open"></i>
            </div>
            <nuxt-link :to="{name: 'admin-post'}" class="small-box-footer">Show <i class="fas fa-arrow-circle-right"></i></nuxt-link>
          </div>
        </div>
        <div class="col-md-3 col-sm-6 col-12">
          <div class="small-box bg-danger">
            <div class="inner">
              <h3>{{ comments }}</h3>
              <p>Comments</p>
            </div>
            <div class="icon">
              <i class="fa fa-comments"></i>
            </div>
            <a href="#" class="small-box-footer">Show <i class="fas fa-arrow-circle-right"></i></a>
          </div>
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
        title: 'Dashboard - CMS',
      }
    },

    async asyncData({ $axios }) {

      //fetching dashboard
      const dashboard = await $axios.$get('/api/admin/dashboard')

      return {
        'users': dashboard.data.users,
        'posts': dashboard.data.posts,
        'categories': dashboard.data.categories,
        'comments': dashboard.data.comments,
        'tags': dashboard.data.tags,
      }
    },

  }
</script>

<style>

</style>