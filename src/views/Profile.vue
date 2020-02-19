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
                Веб-сайт: <a :href="`https://${profile.website}`" target="_blank">{{profile.website}}</a>
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
              <v-dialog v-model="dialog" scrollable max-width="600px" v-if="myId == $route.params.id">
                <template v-slot:activator="{ on }">
                  <v-btn color="green darken-2" dark v-on="on">New Post</v-btn>
                </template>
                <v-card>
                  <v-card-title>
                    <span class="headline">New Post</span>
                  </v-card-title>
                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="12">
                          <v-text-field label="Title*" required v-model="title"></v-text-field>
                        </v-col>
                        <v-col cols="12">
                          <v-text-field label="Text*" required v-model="text"></v-text-field>
                        </v-col>
                      </v-row>
                    </v-container>
                    <small>*indicates required field</small>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="dialog = false">Close</v-btn>
                    <v-btn color="blue darken-1" text @click="newPost(); dialog = false">Add</v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
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
        @click="aBook(post.id)"
      >
        Add to Bookmarks
      </v-btn>
      <v-spacer></v-spacer>
      <div>{{post.likes}}</div>
      <v-btn icon @click="like(post.id)">
        <v-icon>mdi-heart</v-icon>
      </v-btn>
    </v-card-actions>
    </v-card>
    </v-container>
    </div>
</div>
</template>
<script>
export default {
    props: ["myId"],
    data(){
        return {
            profiles: null,
            profile: null,
            posts: [],
            dialog: false,
            title: "",
            text: "",
        }
    },
    watch: {
        $route: {
            handler(){
                console.log("i'm ready")
                this.$axios.get('https://api.myjson.com/bins/8vhwg')
                .then(response=>{
                    console.log('response', response.data)
                    this.profiles = response.data
                    for(let index in this.profiles) {
                      if(this.profiles[index].id == this.$route.params.id) {
                        this.profile = this.profiles[index] 
                        console.log("profile", this.profile)
                      }
                    }
                })
                this.$axios.get('https://api.myjson.com/bins/1bn13k')
                .then(response=>{
                    console.log('response', response.data)
                    for(let index of response.data) {
                      if(index.userId == this.$route.params.id)
                        this.posts.push(index)
                    }
                })
            },
            immediate: true,
        }
    },
    methods: {
      newPost() {
        this.axios.get("https://api.myjson.com/bins/1bn13k")
        .then( (response)=>{
            let posts = response.data;
            let newPost = {
              userId: this.myId,
              id: posts.length + 1,
              title: this.title,
              body: this.text,
              likes: 0,
              whoLiked: [],
              whoAdded: []
            }

            posts.unshift(newPost);
            // posts=[{"userId":2,"id":2,"title":"Заявление от настоящего Марка Цукерберга","body":"Я не люблю фейсбук","likes":0,"whoLiked":[],"whoAdded":[1]},{"userId":1,"id":1,"title":"Я админ","body":"Да, я админ","likes":0,"whoLiked":[],"whoAdded":[1]}]
            this.axios.put('https://api.myjson.com/bins/1bn13k', posts)
            this.posts.unshift(newPost);     
            this.title = '';
            this.text = '';
        })
      },
      like(e) {
        this.axios.get("https://api.myjson.com/bins/1bn13k")
        .then( (response)=>{
          let posts = response.data;
          let num = false;
          if(this.myId != '') {
            for(let index in posts) {
              if(posts[index].id == e) {
                for(let i in posts[index].whoLiked) {
                  if(posts[index].whoLiked[i] == this.myId) {
                    posts[index].likes--;
                    posts[index].whoLiked.splice(i, 1);
                    num = true;
                  }
                }
                if(!num) {
                  posts[index].whoLiked.push(this.myId);
                  posts[index].likes++;
                }
              }
            }
            for(let index in this.posts) {
              for(let i in posts) {
                if(this.posts[index].id == posts[i].id) {
                  this.posts[index].likes = posts[i].likes;
                }
              }
            }
            this.axios.put('https://api.myjson.com/bins/1bn13k', posts)
          }
          else{
            window.alert('Сначала войдите в аккаунт')
          }
        })
      },
      aBook(e) {
        this.axios.get("https://api.myjson.com/bins/1bn13k")
        .then( (response)=>{
          let posts = response.data;
          if(this.myId != '') {
            for(let index in posts) {
              if(posts[index].id == e) {
                posts[index].whoAdded.push(this.myId)
              }
            }
            this.axios.put('https://api.myjson.com/bins/1bn13k', posts)
          }
          else{
            window.alert('Сначала войдите в аккаунт')
          }
        })
      }
    }
}
</script>