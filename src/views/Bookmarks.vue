<template>
<div>
    <v-row style="margin-left: 15px;" class="text-left">
        <v-col cols="10">
            <h1 class="green--text text--darken-2">
                <v-icon large color="green darken-2">mdi-book-outline</v-icon>
                Your Bookmarks
            </h1>
        </v-col>
    </v-row>

    <v-divider></v-divider>

    <div v-if="posts">
      <v-container class="text-left" fluid>
        <v-card
        style="margin: 25px;" 
        width="60vw"
        v-for="(post, index) in posts" :key="post.id"
        >
          <v-list-item>
            <v-list-item-avatar color="grey">
                <v-img :src="profiles[post.userId - 1].photo"></v-img>
            </v-list-item-avatar>
            <v-list-item-content>
              <v-list-item-title class="headline">{{post.title}}</v-list-item-title>
              <v-list-item-subtitle>by {{profiles[post.userId - 1].name}}</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-card-text style="margin-left: 10px">
            {{post.body}}
          </v-card-text>

          <v-card-actions>
            <v-btn
              text
              color="deep-purple accent-4"
              @click="dBook(index)"
            >
              Delete From Bookmarks
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
        posts: [],
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
          })
          this.$axios.get('https://api.myjson.com/bins/1bn13k')
          .then(response=>{
            console.log('response', response.data)
            for(let index of response.data)
              for(let i in index.whoAdded)
                if(index.whoAdded[i] == this.myId)
                  this.posts.push(index)
          })
        },
        immediate: true,
      }
    },
    methods: {
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
      dBook(e) {
        this.axios.get("https://api.myjson.com/bins/1bn13k")
        .then( (response)=>{
          let posts = response.data;
          posts[e].whoAdded.forEach((item, index) => {
            if(item == this.myId) {
              posts[e].whoAdded.splice(index, 1);
            }
          });
          this.posts.splice(e, 1);
          this.axios.put('https://api.myjson.com/bins/1bn13k', posts)
        })
      }
    }
}
</script>