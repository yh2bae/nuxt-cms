<template>

  <div class="hold-transition sidebar-mini">
    <div class="wrapper">
      <!-- Header -->
      <Header />
      <!-- /.Hader -->

      <aside class="main-sidebar sidebar-dark-primary elevation-4">
        <a to="/admin/dashboard" class="brand-link text-center">
          <span class="brand-text font-weight-bold">CMS - ADMINISTRATOR</span>
        </a>

        <!-- Sidebar -->
        <div class="sidebar">
          <Sidebar />
        </div>
        <!-- /.sidebar -->
      </aside>

      <!-- content -->
      <Nuxt />
      <!-- / .content -->

      <footer class="main-footer">
        <strong>Copyright &copy; 2021 CMS.</strong> All rights reserved.
      </footer>
      <aside class="control-sidebar control-sidebar-dark">
      </aside>
    </div>
  </div>
</template>

<script>
  import Header from '@/components/admin/header.vue'
  import Sidebar from '@/components/admin/sidebar.vue'

  export default {

    //middleware auth
    middleware: 'auth',

    components: {
      Header,
      Sidebar
    }

  }
</script>

<style>
  .wrapper, .content-wrapper {
    background-color: #ccdcec !important;
  }
</style>