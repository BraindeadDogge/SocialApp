<template>
    <div class="d-flex justify-center">
        <v-card width="600px" class="mt-12 pa-10">
            <v-card-title>
                Войдите в аккаунт
            </v-card-title>

            <v-text-field
                label="Введите логин"
                v-model="login"
                outlined
            ></v-text-field>

            <v-text-field
                label="Введите пароль"
                v-model="password"
                outlined
                type="password"
            ></v-text-field>

            <v-btn @click="authenticate">
                Войти
            </v-btn>
        </v-card>
    </div>
</template>

<script>
export default {
    data(){
        return {
            login: '',
            password: ''
        }
    },
    methods: {
        authenticate() {
            this.axios.get("https://api.myjson.com/bins/8vhwg")
            .then((response) => {
                console.log("Users: ", response)
                let users = response.data;
                let found = false;
                for(let index in users) {
                    if(this.login == users[index].username && this.password == users[index].password) {
                        this.$emit('login', {
                            id: users[index].id,
                            photoURL: users[index].photo,
                            name: users[index].name,
                        })
                        this.$router.push('/profile/' + users[index].id);
                        found = true;
                        break;
                    }
                }
                if(!found)
                    window.alert('Беги, глупый хацкер!')
            })
        }
    }
}
</script>