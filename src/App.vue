<template>
  <div id="app">
    <nav>
      <div class="nav-wrapper #0d47a1 blue darken-4">
        <a href="/" class="brand-logo"><span class="space-left">Tanda</span></a>
        <ul id="nav-mobile" class="right hide-on-med-and-down">
          <li class="nav-item" v-if="!isLoggedIn()">
            <router-link class="nav-link" to="/login">Login</router-link>
          </li>

          <li class="nav-item" v-if="isLoggedIn()">
            <router-link class="nav-link" to="/logout">Logout</router-link>
          </li>
          <li class="nav-item" v-if="!isLoggedIn()">
            <router-link class="nav-link" to="/signup">Signup</router-link>
          </li>
        </ul>
      </div>
    </nav>
    <router-view />
  </div>
</template>
<script>
export default {
  data: function () {
    return {};
  },
  created: function () {
    this.onSaved();
  },
  methods: {
    isLoggedIn: function () {
      return localStorage.getItem("jwt");
    },
    onSaved: function () {
      return window.location.href === "http://localhost:8080/user_organizations";
    },
  },
  components: {},
};
</script>
<style>
.space-left {
  padding-left: 20px;
}
</style>
