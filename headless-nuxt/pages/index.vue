<template>
    <div v-if="projects">
        <div class="project-list">
        <h1>Portolio</h1>
        <div class="filter-container" v-if="types"></div>
        <div v-if="types" class="my-8">
        Filtrer par projet
        <div class="filter-buttons">
        <button class="filter-button" @click="filterProjects('all')">Tous</button>
        <button  class="filter-button" v-for="type in types" @click="filterProjects(type)" :key="type" >
         {{ type }} </button>
    </div>
    </div>
    <div class="grid">
        <nuxt-link class="project" :to="`projects/${project.slug}`" v-for="project in filteredProjects" :key="project.id" >
            <img :src="project.image.url" alt="project.name" >
            <h2>{{ project.name }}</h2> 
      
            </nuxt-link>
    </div> 
    </div>
</div>
</template>

<script setup>

const { find } = useStrapi()
const projects = ref()
const types = ref([])
const activeFilter = ref('all')

const filterProjects = (type) => {
    activeFilter.value = type
}

const filteredProjects = computed(() => {
if(activeFilter.value === 'all') return projects.value.data
return projects.value.data.filter(project => project.type === activeFilter.value)
})

onMounted( async() => {
    projects.value = await find('projets', {populate: 'deep'})
    types.value = new Set(projects.value.data.map(project => {
        return project.type
    }))

    console.log(projects.value);
})
</script>

<style scoped>
.grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    grid-auto-rows: 300px;
  }

  /* Styles pour les projets individuels */
  .project {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  /* Styles pour les boutons de filtrage */
  .filter-button {
    background-color: #333;
    color: #fff;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: pointer;
  }

  /* Styles pour les titres */
  h1 {
    font-size: 2rem;
    margin-bottom: 1rem;
    text-align: center;
  }

  /* Styles pour les liens */
  a {
    color: #333;
    text-decoration: none;
  }

  /* Styles pour le filtre */
  .filter-container {
    margin-bottom: 1rem;
  }

  /* Styles pour les projets individuels sur mobile */
  @media (max-width: 600px) {
    .grid {
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      grid-auto-rows: 300px;
    }

    .project {
      padding: 0.5rem;
    }
  }
</style>

