<script setup>
import NavBar from './components/NavBar.vue'
import Landing from './Components/Landing.vue'
import Projects from './Components/Projects.vue'
import Tools from './components/Tools.vue'
import Contact from './components/Contact.vue'
import Footer from './components/Footer.vue'


</script>
  
<template>
  <NavBar />
  <div class="container-fluid px-0">
    <Landing />
    <Projects />
    <Tools />
    <Contact />
  </div>
  <Footer />
</template>

<style scoped>

</style>
