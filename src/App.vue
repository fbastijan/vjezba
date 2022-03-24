<template>
  <div id="app">
    <nav id="nav" class="navbar navbar-expand-lg navbar-light">
      <router-link to="/" class="navbar-brand">
        <img
          src="@/assets/instagram.png"
          width="30"
          height="30"
          class="d-inline-block align-top"
          alt=""
        />
        <label>Fipugram </label>
      </router-link>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarToggler"
        aria-controls="navbarToggler"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarToggler">
        <form class="form-inline my-2 my-lg-0 mr-auto ml-auto">
          <input
            class="form-control"
            type="search"
            placeholder="Search"
            aria-label="Search"
            v-model="store.searchTerm"
          />
        </form>
        <ul class="navbar-nav mt-2 mt-lg-0">
          <li v-show="!store.authenticated" class="nav-item">
            <router-link to="/login" class="nav-link">Login</router-link>
          </li>
          <li v-show="!store.authenticated" class="nav-item">
            <router-link to="/signup" class="nav-link">Sign up</router-link>
          </li>
          <li v-show="store.authenticated" class="nav-item">
            <a class="nav-link" href="#" @click.prevent="logout()">Log out</a>
          </li>
        </ul>
      </div>
    </nav>
    <div class="container">
      <router-view />
    </div>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  .nav-link {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
<script>
import { firebase } from "@/firebase";
import store from "@/store.js";

export default {
  data() {
    return {
      store,
    };
  },
  methods: {
    logout() {
      firebase
        .auth()
        .signOut()
        .then(() => {
          this.$router.push({ name: "login" });
        });
    },
  },
};
firebase.auth().onAuthStateChanged((user) => {
  if (user) {
    store.authenticated = true;
    console.log(user.email);

    console.log("current user debugg", store.authenticated);
  } else {
    store.authenticated = false;
    console.log("Nije prijavljen user");
  }
});
</script>
