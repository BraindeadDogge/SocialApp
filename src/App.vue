<template>
  <v-app id="app">
    <v-navigation-drawer color="green"
                         dark
                         right
                         app
                         expand-on-hover
                         hide-overlay
                         permanent>
      <v-list
          nav 
          shaped
          dense>
        <v-list-item two-line>
          <v-list-item-avatar>
            <img src="https://randomuser.me/api/portraits/men/17.jpg" alt="мужчина">
          </v-list-item-avatar>
          <v-list-item-content class="text-left">
            <v-list-item-title class="font-weight-black">SocialWeb</v-list-item-title>
            <v-list-item-subtitle>Ivan Ivanich</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-divider class="my-3"></v-divider>  
        <v-list-item link
                     :to="link.path"
                     :key="link.path"
                     v-for="link in links">
          <v-list-item-icon>
            <v-icon>{{link.icon}}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title class="text-left">
              {{link.label}}
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-content class="px-12 py-3">
      <v-container fluid>
        <router-view/>
      </v-container>
    </v-content>
  </v-app>
</template>
<script>
export default {
  data(){
    return {
      links: [
        {
          label: 'Main',
          path: '/',
          icon: 'mdi-home-outline'
        },
        {
          label: 'Users',
          path: '/users',
          icon: 'mdi-fingerprint'
        },
        // {
        //   label: 'Профиль',
        //   path: '/profile',
        //   icon: 'mdi-face'
        // },
      ]
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
