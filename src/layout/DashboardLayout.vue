<template>
  <div class="wrapper">
    <side-bar>
      <mobile-menu slot="content"></mobile-menu>
      <sidebar-link to="/bom-destino/overview">
        <i class="nc-icon nc-chart-pie-35"></i>
        <p>Dashboard</p>
      </sidebar-link>
      <sidebar-link to="/bom-destino/user">
        <i class="nc-icon nc-circle-09"></i>
        <p>User Profile</p>
      </sidebar-link>
      <sidebar-link to="/bom-destino/table-list">
        <i class="nc-icon nc-notes"></i>
        <p>Lista de medicos</p>
      </sidebar-link>
      <sidebar-link to="/bom-destino/typography">
        <i class="nc-icon nc-paper-2"></i>
        <p>Educação básica</p>
      </sidebar-link>
      <sidebar-link to="/bom-destino/icons">
        <i class="nc-icon nc-atom"></i>
        <p>Serviços</p>
      </sidebar-link>
      </sidebar-link>
      <sidebar-link to="/bom-destino/query">
        <i class="nc-icon nc-pin-3"></i>
        <p>Consultas IPTU/ITR</p>
      </sidebar-link>

    </side-bar>
    <div class="main-panel">
      <top-navbar></top-navbar>

      <dashboard-content @click="toggleSidebar">

      </dashboard-content>

      <content-footer></content-footer>
    </div>
  </div>
</template>
<style lang="scss">

</style>
<script>
  import TopNavbar from './TopNavbar.vue'
  import ContentFooter from './ContentFooter.vue'
  import DashboardContent from './Content.vue'
  import MobileMenu from './MobileMenu.vue'
  export default {
    components: {
      TopNavbar,
      ContentFooter,
      DashboardContent,
      MobileMenu
    },
    methods: {
      toggleSidebar () {
        if (this.$sidebar.showSidebar) {
          this.$sidebar.displaySidebar(false)
        }
      }
    }
  }

</script>
