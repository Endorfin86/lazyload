<template>
        <main>
        <div v-if="articles.length > 0">
            <div v-for="el in articles" class="post">
                <button @click="delPost(el.id)"><i class="fa-solid fa-trash-can"></i></button>
                <img alt="Фото" width="200" :src="el.photo">
                <h2>{{ el.title }}</h2>
                <p>{{ el.content }} {{ el.text }}</p>
            </div>

        </div>
        <div v-else>
            <h2>Нет постов</h2>
        </div>
    </main>
    <aside>
        <AddPost v-on:add-post="createPost"/>
    </aside>

</template>

<script>
import AddPost from './AddPost.vue'
import axios from 'axios'

export default {
    name: 'Main',
    created () {
        window.addEventListener('scroll', this.loadPosts);
    },
    data() {
        return {
            height: window.innerHeight,
            articles: [],
            posts: []
        }
    },
    components: {
        AddPost,
    },
    methods: {
        delPost(id) {
            this.articles = this.articles.filter(i => i.id !== id)
            axios.delete(`http://127.0.0.1:3000/api/v1/womenlist/${id}`)
        },
        createPost(titleInput, textInput) {
            this.articles.unshift({
                id: Math.random() * 1000,
                title: titleInput,
                text: textInput,
            })
        },
        async loadPosts() {
            const scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
            const scrollHeight = document.documentElement.scrollHeight || document.body.scrollHeight;
            const windowHeight = document.documentElement.clientHeight || window.innerHeight;
            
            if(scrollTop + windowHeight >= scrollHeight - 100) {
                const len = this.articles.length + 2
                const res = await axios.get('http://127.0.0.1:3000/api/v1/womenlist/?format=json')
                this.articles = res.data.slice(0, len);
            }
            }
    },
    async mounted() {
        try {
            const res = await axios.get('http://127.0.0.1:3000/api/v1/womenlist/?format=json')
            this.articles = res.data.slice(0, 3);
        } catch(error) {
            console.log(error);
        }
    },
}
</script>

<style scoped>
main {
    padding: 20px;

    min-height: 100vh;
    width: 70%;
    float: left;
    padding-bottom: 100px;
}
aside {
    padding: 20px;
    height: 100vh;
    width: 30%;
    float: left;
}
.post {
    background-color: rgb(255, 255, 255);
    padding: 20px;
    margin-bottom: 20px;
    border-radius: 5px;
    width: 100%;
    float: left;
    animation-name: identifier;
    animation-duration: .5s;
    animation-delay: 1s;
    animation-fill-mode: both;
    opacity: 0;
}

.post button {
    float: right;
    border: 0px;
    background-color: rgba(240, 248, 255, 0);
}
.post button:hover {
    cursor: pointer;
    color: brown;
}

.post img {
    float: left;
    margin-right: 20px;
}


@keyframes identifier {
    0% {}
    100% {opacity: 1;}
}
</style>