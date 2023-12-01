<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="deletHandler" @complete="completeHandle"/>
      </div>
    </div>
  </div>
</template>

<script>
import FilterNav from '@/components/FilterNav.vue';
import SingleProject from "../components/SingleProject.vue";
export default {
  name: 'HomeView',
  components: {SingleProject, FilterNav},
  data(){
    return{
      projects: [],
      current:'all'
    }
  },
  mounted(){
    fetch("http://localhost:3000/posts")
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message));
  },
  methods: {
    deletHandler(id){
      this.projects = this.projects.filter(project => project.id !== id);
    },
    completeHandle(id){
      let chngcmplte = this.projects.find(item =>  item.id === id);
      chngcmplte.complete = !chngcmplte.complete
    }
  },
  computed: {
    filteredProjects(){
      if(this.current === 'completed'){
        return this.projects.filter(item => item.complete)
      } else if(this.current === 'ongoing') {
        return this.projects.filter(item => !item.complete)
      } else {
        return this.projects
      }
    }
  }
}

</script>
