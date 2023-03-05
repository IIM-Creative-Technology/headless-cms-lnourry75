<template>
   <div v-if="project">
  <div class="project-page">
    <div class="container">
    <button class="back-btn" @click="goBack()">Retour</button>
    <div class="project-header">
        <h1>{{ project.name }}</h1>
        <img :src="project.image.url" alt="" width="500">
      </div>
      <div class="project-description">
        <h2>Description du projet</h2>
        <p>{{  project.description }}</p>
        <p><a :href="project.link">Lien vers le projet</a></p>
        </div>
        <div class="project-technologies">
        <h2>Technologies utilisées</h2>
        <div v-for='technologie in project.technologies' :key="technologie.id" class="tech-item"> 
          <img :src="technologie.image.url" alt="" width="100">
          <h3>{{ technologie.name }}</h3>
     </div>
        </div>
    </div>
  </div>
  </div>
</template>

<script setup>
const { findOne } = useStrapi()
const route = useRoute()
const project = ref()

onMounted( async () => {
    project.value = await findOne( `projets?filters[slug]=${route.params.slug}&populate=deep`)
    project.value = project.value.data[0]
    console.log(project.value)
})


</script>

<script>
export default {
  methods: {
    goBack() {
      window.history.back();
    }
  }
}
</script>


<style>
  .project-page {
    background-color: #F2F2F2;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .container {
    max-width: 960px;
    margin: 0 auto;
    padding: 50px;
    background-color: #FFFFFF;
    border-radius: 10px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  }

  .back-btn {
    background-color: #F5A623;
    color: #FFFFFF;
    font-size: 18px;
    border-radius: 5px;
    padding: 10px 20px;
    border: none;
    margin-bottom: 20px;
    cursor: pointer;
  }

  .project-header {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 50px;
  }

  .project-header h1 {
    font-size: 48px;
    font-weight: bold;
    margin-bottom: 20px;
  }

  .project-description {
    margin-bottom: 50px;
  }

  .project-description h2 {
    font-size: 36px;
    font-weight: bold;
    margin-bottom: 20px;
  }

  .project-description p {
    font-size: 18px;
    line-height: 1.5;
    margin-bottom: 10px;
  }

  .project-description a {
    color: #F5A623;
    text-decoration: none;
    font-weight: bold;
  }

  .project-technologies {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }

  .tech-item {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
    width: 48%;
  }

  .tech-item img {
    margin-right: 20px;
  }

  .project-technologies h2 {
    font-size: 36px;
    font-weight: bold;
    margin-bottom: 20px;
    width: 100%;
  }


</style>
