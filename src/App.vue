<template>
  <v-app id="app">
    <v-navigation-drawer
        color="green"
        dark
        expand-on-hover
        hide-overlay
        permanent
        right
        app
      >

        <v-list
          nav 
          shaped
          dense
          v-if="user.myId != ''"
        >
          <v-list-item two-line>
            <v-list-item-avatar>
              <img :src="user.photoURL">
            </v-list-item-avatar>

            <v-list-item-content class="text-left">
              <v-list-item-title class="font-weight-black">SocialApp</v-list-item-title>
              <v-list-item-subtitle>{{user.name}}</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
          <v-divider class="my-3"></v-divider>
          <v-list-item link to="/">
            <v-list-item-icon>
              <v-icon>mdi-home-outline</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="text-left">Main</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item link :to="'/profile/' + user.myId">
            <v-list-item-icon>
              <v-icon>mdi-account-outline</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="text-left">My profile</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item link to="/users" exact>
            <v-list-item-icon>
              <v-icon>mdi-account-multiple-plus-outline</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="text-left">Users</v-list-item-title>            
            </v-list-item-content>
          </v-list-item>
          <v-list-item link to="/bookmarks" exact>
            <v-list-item-icon>
              <v-icon>mdi-book-outline</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="text-left">Your Bookmarks</v-list-item-title>            
            </v-list-item-content>
          </v-list-item>

          <v-divider class="my-3"></v-divider>

          <v-list-item link @click="user.myId=''">
            <v-list-item-icon>
              <v-icon>mdi-account-arrow-right-outline</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="text-left">Sign out</v-list-item-title>            
            </v-list-item-content>
          </v-list-item>
        </v-list>

        <v-list
          nav 
          shaped
          dense
          v-else
        >
          <v-list-item link to="/login" exact>
            <v-list-item-icon>
              <v-icon>mdi-account-arrow-left-outline</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="text-left">Sign in</v-list-item-title>            
            </v-list-item-content>
          </v-list-item>

          <v-list-item link to="/registration" exact>
            <v-list-item-icon>
              <v-icon>mdi-account-plus-outline</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="text-left">Sign up</v-list-item-title>            
            </v-list-item-content>
          </v-list-item>
        </v-list>

      </v-navigation-drawer>
    <v-content class="px-12 py-3">
      <v-container fluid>
        <router-view v-on:login="updateUser" :myId="user.myId"/>
      </v-container>
    </v-content>
  </v-app>
</template>
<script>
export default {
  data(){
    return {
      user: {
        myId: "",
        name: "Undefined",
        photoURL: "https://infokava.com/uploads/posts/2017-05/1495025256_1444399930_image.jpeg"
      }
    }
  },
  methods: {
    updateUser(data) {
      this.user.name = data.name;
      this.user.myId = data.id;
      this.user.photoURL = data.photoURL;
    }
  }
}
</script>
<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
