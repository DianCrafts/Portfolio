<template>
  <div class="main">
    <div class="background-container">
      <img class="background-img" src="./assets/roadmap.png" alt="background">
      <UniversityComponent />
      <WorkComponent />
      <ProjectComponent />
      <ContactInfo />
    </div>
  </div>
</template>


<script>
import ContactInfo from './components/ContactInfo.vue';
import ProjectComponent from './components/ProjectComponent.vue';
import UniversityComponent from './components/UniversityComponent.vue';
import WorkComponent from './components/WorkComponent.vue';

export default {
  components: {
    UniversityComponent,
    ProjectComponent,
    WorkComponent,
    ContactInfo
  }
};
</script>

<style scoped>
html ,body {
  height: 100%;
  margin: 0;
  /* padding: 0; */
  
}




.main {
  padding-top: 10vh;  
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 200vh;
  background-color: #F5F1F0;
  
}

.background-container{
  width: fit-content;
  height: fit-content;
  position: relative;
}


.background-img {
  width: 50vw;
  height: auto;
}
/* .university-container {
  width: 10%;
  height: fit-content;
  position: absolute;
  top: 5%;
  right: 70%;

}  */




</style>