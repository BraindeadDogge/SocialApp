<template>
<div v-if="profile">
    <v-row style="margin-left: 15px;" class="text-left">
        <v-col cols="10">
            <h1 class="green--text text--darken-2">
                <v-icon large color="green darken-2">mdi-account-outline</v-icon>
                {{profile.name}}
            </h1>
        </v-col>
    </v-row>
    <v-row style="margin-left: 20px;" class="text-left">
        <v-col cols="2">
            <img :src="profile.photo" style="max-width: 100%">
        </v-col>
        <v-col cols="10" class="text-left">
            <p>
                Веб-сайт: <a :href="profile.website" target="_blank">{{profile.website}}</a>
            </p>
            <p>
                E-mail: <a :href="`mailto:${profile.email}`">{{profile.email}}</a>
            </p>
            <p>
                Город: {{profile.address.city}}
            </p>
            <p>
                Место работы: {{profile.company.name}}
            </p>
        </v-col>
    </v-row>

    <v-divider></v-divider>

    <div v-if="posts">
    <v-container class="text-left" fluid>
    <v-card
    style="margin: 25px;" 
    width="60vw"
    v-for="post in posts" :key="post.id"
    >
    <v-list-item>
      <v-list-item-avatar color="grey">
          <v-img :src="profile.photo"></v-img>
      </v-list-item-avatar>
      <v-list-item-content>
        <v-list-item-title class="headline">{{post.title}}</v-list-item-title>
        <v-list-item-subtitle>by {{profile.name}}</v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>

    <v-card-text style="margin-left: 10px">
      {{post.body}}
    </v-card-text>

    <v-card-actions>
      <v-btn
        text
        color="deep-purple accent-4"
      >
        Read
      </v-btn>
      <v-btn
        text
        color="deep-purple accent-4"
      >
        Bookmark
      </v-btn>
      <v-spacer></v-spacer>
      <v-btn icon>
        <v-icon>mdi-heart</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>mdi-share-variant</v-icon>
      </v-btn>
    </v-card-actions>
    </v-card>
    </v-container>
    </div>
</div>
</template>
<script>
export default {
    data(){
        return {
            profile: null,
            posts: null,
        }
    },
    watch: {
        $route: {
            handler(){
                console.log("i'm ready")
                this.$axios.get('http://188.225.47.187/api/jsonstorage/230a2ba25dd93eadc4d15a3a8c57cd92')
                .then(response=>{
                    console.log('response', response.data)
                    this.profile = response.data
                    for(let index in this.profile) {
                      if(this.profile[index].id === this.$route.params.id)
                        this.profile = this.profile[index] 
                    }
                })
                this.$axios.get('http://jsonplaceholder.typicode.com/posts?userId='+ this.$route.params.id)
                .then(response=>{
                    console.log('response', response.data)
                    this.posts = response.data
                })
            },
            immediate: true,
        }
    }
}
</script>