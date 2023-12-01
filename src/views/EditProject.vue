<template>
    <form @submit.prevent="subHandle">
        <label>Title</label>
        <input v-model="title" type="text" required>
        <label>Details</label>
        <textarea v-model="details"></textarea>
        <button>Update Project</button>
    </form>
</template>

<script>
export default {
    props: ['id'],
    data(){
        return{
            title: '',
            details: '',
            url: 'http://localhost:3000/posts/' + this.id,
        }
    },
    mounted(){
        fetch(this.url)
        .then(res => res.json())
        .then(data => {
            this.title = data.title;
            this.details = data.author;
        })
        .catch(err => console.log(err.message))
    },
    methods: {
        subHandle(){
            fetch(this.url, {
                method: 'PATCH',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify({
                    title: this.title,
                    author: this.details
                })
            })
            .then(() => this.$router.push('/'))
            .catch(err => console.log(err.message))
        }
    }
}
</script>

<style>
    form{
        background: white;
        padding: 20px;
        border-radius: 10px;
    }
    label{
        display: block;
        color: #bbb;
        text-transform: uppercase;
        font-size: 14px;
        font-weight: bold;
        letter-spacing: 1px;
        margin: 20px 0 10px 0;
    }
    input{
        padding: 10px;
        border: none;
        border-bottom: 1px solid #bbb;
        width: 100%;
        box-sizing: border-box;
    }
    textarea{
        border: 1px solid #ddd;
        padding: 10px;
        width: 100%;
        box-sizing: border-box;
        height: 100%;
    }
    form button{
        display: block;
        margin: 20px auto 0;
        background: #00ce89;
        color: white;
        padding: 10px;
        border: 0;
        border-radius: 6px;
        font-size: 16px;
    }
</style>