<template>
  <header class="app-header navbar d-flex justify-content-start">
    <button class="navbar-toggler mobile-sidebar-toggler d-lg-none" type="button" @click="mobileSidebarToggle">
      <span class="navbar-toggler-icon"></span>
    </button>
    <b-link class="navbar-brand" to="#"></b-link>
    <button class="navbar-toggler sidebar-toggler d-md-down-none" type="button" @click="sidebarToggle">
      <span class="navbar-toggler-icon"></span>
    </button>
    <b-navbar-nav class="d-md-down-none">
      <b-nav-item class="px-3">Ir para loja</b-nav-item>
    </b-navbar-nav>

    <b-navbar-nav class="ml-auto">
      <template v-if="authenticated">
        <b>Logado como:</b>
          <b-nav-item-dropdown class="px-4">
              <template slot="button-content">
                  {{user.name}}
              </template>
              <b-dropdown-item @click.prevent="signOut">Deslogar</b-dropdown-item>
          </b-nav-item-dropdown>
      </template>
      <template v-else>
        <div  class="px-4 d-flex">
          <li class="nav-item">
              <nuxt-link class="nav-link" to="/dashboard/login">Logar</nuxt-link>
          </li>
          <li class="nav-item">
              <nuxt-link class="nav-link" to="/dashboard/login/registrar">Registrar</nuxt-link>
          </li>
        </div>
      </template>
    </b-navbar-nav>

    <!-- <b-navbar-nav class="ml-auto">
      <b>Logado como:</b>
      <b-nav-item class="px-3">{{user.name}}</b-nav-item>
      <b-nav-item class="px-3"> {{authenticated}}</b-nav-item>

    </b-navbar-nav> -->
    <!-- <b-navbar-nav class="ml-auto">
      <b-nav-item class="d-md-down-none">
        <i class="icon-bell"></i>
        <b-badge pill variant="danger">5</b-badge>
      </b-nav-item>
      <b-nav-item class="d-md-down-none">
        <i class="icon-list"></i>
      </b-nav-item>
      <b-nav-item class="d-md-down-none">
        <i class="icon-location-pin"></i>
      </b-nav-item>
      <HeaderDropdown/>
    </b-navbar-nav> -->
    <!-- <button class="navbar-toggler aside-menu-toggler d-md-down-none" type="button" @click="asideToggle">
      <span class="navbar-toggler-icon"></span>
    </button> -->
  </header>
</template>

<script>
  import HeaderDropdown from './HeaderDropdown.vue'

  export default {
    name: 'c-header',
    components: {
      HeaderDropdown
    },
    methods: {
      signOut() {
        this.$auth.logout();
        this.$router.push({name: 'dashboard/login'});
      },
      sidebarToggle (e) {
        e.preventDefault()
        document.body.classList.toggle('sidebar-hidden')
      },
      sidebarMinimize (e) {
        e.preventDefault()
        document.body.classList.toggle('sidebar-minimized')
      },
      mobileSidebarToggle (e) {
        e.preventDefault()
        document.body.classList.toggle('sidebar-mobile-show')
      },
      asideToggle (e) {
        e.preventDefault()
        document.body.classList.toggle('aside-menu-hidden')
      }
    }
  }
</script>
