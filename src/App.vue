<template>
  <div id="app">
    <nav class="navbar fixed-top navbar-expand-lg navbar-dark bg-dark navbar-dark-hover-color:green">
      <div class="container-fluid">
        <router-link class="navbar-brand" to="/">J.Whis Films</router-link>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <router-link class="nav-link active" to="/movies/new">
                <button type="button" class="btn btn-outline-light">Create Movie</button>
              </router-link>
            </li>
            <div v-if="isLoggedIn()">
              <li class="nav-item">
                <router-link class="nav-link active" to="/logout">
                  <button type="button" class="btn btn-outline-light">Logout</button>
                </router-link>
              </li>
            </div>
            <div v-else>
              <div>
                <li class="nav-item">
                  <router-link class="nav-link active" to="/signup">
                    <button type="button" class="btn btn-outline-light">Sign Up</button>
                  </router-link>
                </li>
              </div>
              <div>
                <li class="nav-item">
                  <router-link class="nav-link active" to="/login">
                    <button type="button" class="btn btn-outline-light">Login</button>
                  </router-link>
                </li>
              </div>
            </div>
          </ul>
        </div>
      </div>
    </nav>
    <router-view />
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

<script>
export default {
  methods: {
    isLoggedIn: function () {
      return localStorage.getItem("jwt");
    },
  },
};
</script>
