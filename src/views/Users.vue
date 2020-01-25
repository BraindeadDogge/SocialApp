<template>
<div v-if="profiles">
    <v-row style="margin-left: 15px;" class="text-left">
        <v-col cols="10">
            <h1 class="green--text text--darken-2">
                <v-icon large color="green darken-2">mdi-fingerprint</v-icon>
                Users
            </h1>
        </v-col>
    </v-row>
    <v-container class="fill-height" fluid>
      <v-card v-for="profile in profiles" :key="profile.id" style="margin: 10px;" width="40vw" hover>
        <v-list-item dense>
          <v-list-item-avatar size="150" color="grey">
            <v-img :src="profile.photo"></v-img>
          </v-list-item-avatar>

          <v-list-item-content>
            <div class="overline mb-4">{{profile.username}}</div>
            <v-list-item-title class="headline mb-1">{{profile.name}} <p/></v-list-item-title>
            <v-list-item-subtitle>{{profile.website}} <p> {{profile.phone}} </p></v-list-item-subtitle>
            <v-card-actions>
             <v-btn block text :to="`/profile/${profile.id}`">Check profile</v-btn>
            </v-card-actions>
          </v-list-item-content>
        </v-list-item>
      </v-card>
    </v-container>
</div>
</template>
<script>
export default {
    data(){
        return {
            profiles: null,
        }
    },
    watch: {
        $route: {
            handler(){
                console.log("i'm ready")
                this.$axios.get('http://188.225.47.187/api/jsonstorage/230a2ba25dd93eadc4d15a3a8c57cd92')
                .then(response=>{
                    console.log('response', response.data)
                    this.profiles = response.data
                })
            },
            immediate: true,
        }
    }
}
</script>