<template>
    <div class="header">
        <span><b><a class="logo" href="/">{{ title }}</a></b></span>
        <span v-if="this.Rating > 5" class="rating">Ваш рейтинг больше 5</span>
        <span class="user">{{ fullName }} ({{ Rating }}) <button @click="addRating">&ensp;+&ensp;</button> <button @click="downRating">&ensp;-&ensp;</button></span>
    </div>
</template>

<script>
export default {
    name: 'Header',

    data() {
        return {
            ratingWatch: 0,
            user: {
                name: 'Alex',
                surname: 'Black',
                rating: 0,
                email: 'alexblack@yandex.ru'
            }
        }
    },
    mounted(){
        this.addRating()
    },
    props: {
        title: {}
    },
    computed: {
        fullName() {
            return `${this.user.name} ${this.user.surname}`
        },
        Rating() {
            return `${this.user.rating}`
        }
    },
    methods: {
        addRating() {
            this.user.rating++
            this.ratingWatch++
        },
        downRating() {
            this.user.rating--
            this.ratingWatch--
        }
    },
    watch: {
        ratingWatch(n, o) {
            if(this.ratingWatch > 5) {
                console.log('Ваш рейтинг больше 5')
            }
        }
    }
}

</script>

<style scoped>
.header {
    padding: 15px;
    background-color: rgb(243, 122, 9);
    color: aliceblue;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.logo {
    color: white;
    text-decoration: none;
}
.logo:hover {
    color: rgb(255, 223, 187);
}

</style>