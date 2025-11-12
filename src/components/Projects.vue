<template>
	<section id="projects" class="mx-0 projects-section">
    <div class="container">
      <div class="text-center mb-5">
        <h2 class="pt-4 text-white section-title" >My Projects</h2>
        <p class="text-white section-subtitle">
          A collection of my development work and side projects
        </p>
      </div>
      <div class="row g-4 pb-4">
        <ProjectCard v-for="project in projects" :key="project.id" :project="project" />
      </div>
    </div>
  </section>
</template>
<script setup>
  import ProjectCard from './ProjectCard.vue';
  import projects from '../data/projects.json';
  console.log(projects)
</script>

<style scoped>
  section {
    background-color: #0f172a !important;
  }
</style>
