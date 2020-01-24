<template>
<div v-if="profile">
    <v-row class="text-left">
        <v-col cols="10">
            <h1 class="green--text text--darken-2">
                <v-icon large color="green darken-2">mdi-account-outline</v-icon>
                {{profile.name}}
            </h1>
        </v-col>
    </v-row>
    <v-row class="text-left">
        <v-col cols="2">
            <img :src="`https://randomuser.me/api/portraits/men/${profile.id}.jpg`" style="max-width: 100%">
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

    ОСТАЛЬНОЕ СОДЕРЖИМОЕ СТРАНИЦЫ
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
                this.$axios.get('http://jsonplaceholder.typicode.com/users/'+ this.$route.params.id)
                .then(response=>{
                    console.log('response', response.data)
                    this.profile = response.data
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