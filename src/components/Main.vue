<template>
        <main>
        <div v-if="articles.length > 0">
            <div v-for="el in posts" class="post">
                <button @click="delPost(el.id)"><i class="fa-solid fa-trash-can"></i></button>
                <img alt="Фото" width="200" :src="el.photo">
                <h2>{{ el.title }}</h2>
                <p>{{ el.content }} {{ el.text }}</p>
            </div>
            <button class="btn" v-if="this.update == true" @click="updatePosts"><i class="fa-solid fa-rotate-right"></i> Проверить обновления</button><br>
            <p class="textupdt" v-if="this.noupdate == true"><i class="fa-regular fa-circle-xmark"></i> Обновлений нет</p>
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
    // функция отслеживает любой малейший скроллинг страницы и запускает функцию loadPosts
    created () {
        window.addEventListener('scroll', this.loadPosts);
    },
    // переменные 
    data() {
        return {
            update: false,
            noupdate: false,
            height: window.innerHeight,
            articles: [],
            posts: []
        }
    },
    components: {
        AddPost,
    },
    methods: {
        // удаление поста из базы по api и из массива articles
        delPost(id) {
            this.posts = this.posts.filter(i => i.id !== id)
            axios.delete(`http://127.0.0.1:3000/api/v1/womenlist/${id}`)
            const indexToDelete = this.posts.findIndex(obj => obj.id === id);
            this.articles.splice(indexToDelete, 1);


        },
        // создание локального поста, без отправки по api в базу данных
        createPost(titleInput, textInput) {
            this.articles.push({
                id: Math.random() * 1000,
                title: titleInput,
                text: textInput,
            })
        },
        // подгрузка двух постов из массива articles в массив posts при достижении конца страницы
        async loadPosts() {
            const scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
            const scrollHeight = document.documentElement.scrollHeight || document.body.scrollHeight;
            const windowHeight = document.documentElement.clientHeight || window.innerHeight;
            
            if(scrollTop + windowHeight >= scrollHeight - 100) {
                if(this.posts.length !== this.articles.length) {
                    this.update = false
                    this.noupdate = false
                    const len = this.posts.length + 2
                    this.posts = this.articles.slice(0, len);
                } else {
                    this.update = true
                }
            }
        },
        // проверка на наличие новых записей в базе данных
        async updatePosts() {
            const res = await axios.get('http://127.0.0.1:3000/api/v1/womenlist/?format=json') 
            this.articles = res.data;
            if(this.posts.length !== this.articles.length) {
                this.update = false
                this.noupdate = false
                const len = this.posts.length + 2
                this.posts = this.articles.slice(0, len);
            } else {
                this.noupdate = true
            }
        },  
    },
    // отправка GET-запроса на получение сырых данных из базы данных при загрузку компонента
    async mounted() {
        try {
            const res = await axios.get('http://127.0.0.1:3000/api/v1/womenlist/?format=json')
            this.articles = res.data;
            this.posts = res.data.slice(0, 3);
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

.textupdt {
    width: 20%;
    margin: 0px 40%;
    text-align: center;
    color: brown; 
}

.btn {
    width: 20%;
    margin: 10px 40%;
    padding: 10px;
    border: 1px solid rgb(173, 173, 173);
    color: rgb(173, 173, 173);
    border-radius: 5px;
    background-color: rgba(240, 248, 255, 0);
    text-align: center;
    animation-name: identifier;
    animation-duration: .5s;
    animation-delay: 1s;
    animation-fill-mode: both;
    opacity: 0;  
}
.btn:hover {
    color: rgb(255, 255, 255);
    background-color: brown;
    border: 1px solid brown;
    cursor: pointer;
}
.btn:active{
    background-color: rgb(216, 24, 24);
    border: 1px solid rgb(216, 24, 24);
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