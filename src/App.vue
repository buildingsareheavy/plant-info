<template>
  <v-app>
    <v-toolbar dark fixed id="nav">
      <v-spacer></v-spacer>
      <v-toolbar-items>
        <v-btn flat>
          <router-link to="/">Home</router-link>
        </v-btn>
        <v-btn flat>
          <router-link to="/about">About</router-link>
        </v-btn>
      </v-toolbar-items>
    </v-toolbar>
    <v-container>
      <router-view/>
    </v-container>
  </v-app>
</template>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=IBM+Plex+Mono:500,500i,700|Playfair+Display:900&display=swap");
#app {
  font-family: "IBM Plex Mono", monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding-top: 68px; // toolbar is 48px + 20 extra for padding
  background: #b2dfdb;
}
#nav {
  a {
    color: white;
    text-decoration: none;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
.bold {
  font-weight: bold;
}
.italic {
  font-style: italic;
}
</style>
