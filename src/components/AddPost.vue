<template>
    <form @submit.prevent="createPost" :class="{ error: textLen > 20 }">
        <label for="title">Название</label>
        <input id="title" v-model="titleInput" placeholder="Введите название">
        <label for="text">Описание <span>{{ textLen }} / 20</span></label>
        <textarea id="text" v-model="textInput" placeholder="Введите описание"></textarea>
        <button :disabled="textLen > 20">Отправить</button>
    </form>
</template>

<script>
export default {
    name: 'AddPost',
    data() {
        return {
            titleInput: "",
            textInput: "",
        }
    },
    methods: {
        createPost() {
            if(this.titleInput !== "" && this.textInput !== "") {
                this.$emit('add-post', this.titleInput, this.textInput)
                this.titleInput = ""
                this.textInput = ""
            }
        }
    },
    computed: {
        textLen() {
            return this.textInput.length
        }
    }
}
</script>

<style scoped>
form {
    background-color: rgb(255, 255, 255);
    padding: 20px;
    border-radius: 5px;
}

form.error {
    color: red;
}

form > input, textarea {
    display: block;
    width: 100%;
    padding: 5px;
    margin-bottom: 10px;
    outline: none;
}
form textarea {
    resize: vertical;
    min-height: 100px;
    max-height: 300px;
}

form button {
    margin-top: 10px;
}
</style>