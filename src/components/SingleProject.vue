<template>
  <div class="project" :class="{complete: project.complete}">
    <div class="actions">
        <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
        <div class="icons">
            <router-link :to="{name: 'EditProject', params: { id: project.id }}">
                <span><i class="fa-regular fa-pen-to-square"></i></span>
            </router-link>
            <span @click="deleteHandler"><i class="fa-solid fa-trash"></i></span>
            <span @click="completeHandler"><i class="fa-solid fa-check"></i></span>
            <span></span>
            <span></span>
        </div>
    </div>
    <div class="details" v-if="showDetails">
        <p>{{ project.author }}</p>
    </div>
  </div>
</template>

<script>
export default {
    props: ['project'],
    
    data(){
        return{
            showDetails: false,
            url: "http://localhost:3000/posts/" + this.project.id
        }
    },
    methods: {
        deleteHandler(){
            fetch(this.url, {method: 'delete'})
            .then(() => this.$emit('delete', this.project.id))
            .catch(err => console.log(err.message))
        },
        completeHandler(){
            fetch(this.url, {
                method: 'PATCH',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify({complete: !this.project.id})})
                .then(()=> this.$emit('complete', this.project.id))
                .catch(err => console.log(err.message))
        }
    }
}
</script>

<style>
    .project {
        margin: 20px auto;
        background: #fff;
        padding: 10px 20px;
        border-radius: 4px;
        box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.5);
        border-left: 4px solid #ea0004;
    }
    h3{
        cursor: pointer;
    }
    .actions{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .fa-solid, .fa-regular {
        font-size: 24px;
        margin-left: 10px;
        color: #777;
        cursor: pointer;
    }
    .fa-regular:hover {
        color: #bbb;
    }
    .fa-solid:hover {
        color: #bbb;
    }
    .complete{
        border-left: 4px solid #00ea14;
    }
</style>