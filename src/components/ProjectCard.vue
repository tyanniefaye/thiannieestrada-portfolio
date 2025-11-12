<template>
        <div class="col-md-6 col-lg-4 d-flex">
          <a class="project-card flex-fill" :href="project.link" target="_blank">
            <img
              :src="project.image"
              :alt="project.alt"
              class="project-img"
            />
            <div class="project-body">
              <h3 class="project-title">{{project.title}}</h3>
              <p class="project-desc">{{project.description}}</p>
            </div>
          </a>
        </div>
</template>
<script setup>
	import { defineProps } from 'vue';

defineProps({
    project: Object
})
	
</script>