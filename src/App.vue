<template>
  
  <div class="main-background" :style="{backgroundColor: mainBackgroundcolor}" >
    <div class = "intro">
      <div class ="welcome">Welcome!</div>
      <div class="info" > 
        <p>here you should see short info about me <br>
          for instance the face that I am beautiful<br>
          and also cute and smart ^^
        </p>
      </div>
    </div>
  </div>
  <div class ="img-container">
        <img class="img" src="./assets/sticky part.png">
  </div>

  <div class="second-background" :style="{ backgroundColor: secondBackgroundColor}" @scroll="handleScroll">
    <div class = "desc-container">
       <div class ="title" :style="{opacity: opacity}" ref="fadeElement">Welcome!</div>
      <div class="detail" :style="{opacity: opacity }" >  
        <p>here you should see short info about me <br>
          for instance the face that I am beautiful<br>
          and also cute and smart ^^
        </p>
      </div>
    </div>
  </div>

   
  
  
</template>


<script>
export default {
  data() {
    return {
      mainBackgroundcolor: "#F0F3F8",
      secondBackgroundColor: "#0F172A",
      opacity: 0,
    };
  },
  mounted() {
    // Attach the scroll event listener to the window or specific container
    window.addEventListener("scroll", this.handleScroll);
  },
  beforeUnmount() {
    // Remove the scroll event listener when component is destroyed
    window.removeEventListener("scroll", this.handleScroll);
  },
  methods: {
    handleScroll() {
      const element = this.$refs.fadeElement;
      const rect = element.getBoundingClientRect();
      const startTriggerPoint = window.innerHeight ;
      const maxOpacityPoint = window.innerHeight * 0.50;
      if (rect.top > startTriggerPoint) {
        this.opacity = 0;
      } 
      else if (rect.top <= startTriggerPoint && rect.top >= maxOpacityPoint) {
        this.opacity = (startTriggerPoint - rect.top) / (startTriggerPoint - maxOpacityPoint);
      } 
      else if (rect.top < maxOpacityPoint) {
        const fadeOutStart = maxOpacityPoint;
        const fadeOutEnd = window.innerHeight * 0.1;
        this.opacity = Math.max(0, 1 - (fadeOutStart - rect.top) / (fadeOutStart - fadeOutEnd));
      }
    }
  },
}
</script>


<style scoped>

html, body {
  height: 100%; 
  margin: 0;
  overflow-y: scroll; 
}

.main-background {
  display: flex;
  width: 100%;
  justify-content: center;
  /* overflow-y: scroll; */
  position: relative; 
  height: 70vh; 
}
.intro{
  display: flex;
  justify-content: center;
  width: 70%;
  margin: auto;
  
}

.welcome {

  margin-right: 5%;
  font-size: 10dvh;
}
.info{
  border-left: black solid;
  font-size: 5dvh
}


.second-background {
  display: flex;
  justify-content: center;
  position: relative;
  height: 300vh;
  width: 100%;  
  /* overflow: visible; */
  padding-top: 20%;
  
}



.desc-container{
  /* margin-right: 10%; */
  display: flex;
  height: fit-content;
  position: absolute; 
  top:20%;          
  /* left: 50%;   */
  /* justify-content: center; */
  width: fit-content;
  z-index: 10;

}
.title {
  margin-right: 5%;
  color: white;
  height: 3%;
  font-size: 10rem;
}
.detail{
  border-left: white solid;
  color: white;
  height: 5%;
  font-size: 5rem

}

.img-container{
  margin : 0;
  height: fit-content;
  width: 100%;
  top: 0;
  margin-bottom: -10px;
  position: sticky;
  z-index: 10;
}
.img{
  width: 100%;
}
/* .intro{
  display: flex;
  justify-content: center;
  width: 100%;
  font-size: 20cap;
  height: 71.3%;
  position: relative;
}
.img-container{
  margin: auto;
  height: fit-content; */
  /* position: absolute;
  top: 59%; */
  /* right: 12%; */
  /* z-index: 10;
} */
/* .data-top-part{
  width: 60%;
  position: relative
} */

/* .sticky-img{
  width: 100%;
} */


</style>

<!-- <template>
  
  <div class="main-background" :style="{backgroundColor: mainBackgroundcolor}">
  <div class="second-background" :style="{ backgroundColor: secondBackgroundColor,top: secondBackgroundTop + '%' }"></div>
  </div>
  
</template>


<script>
export default {
  data() {
    return {
      mainBackgroundcolor: "#F0F3F8",
      secondBackgroundTop: 40,
    };
  },
  mounted() {

    // Add scroll event listener
    window.addEventListener('scroll', this.handleScroll);
  },
  methods: {
    handleScroll() {
      const scrollTop = window.scrollY;
      const maxScroll = document.documentElement.scrollHeight - window.innerHeight;
      let newTop = 40 - (scrollTop / maxScroll) * 40;
      this.secondBackgroundTop = Math.max(0, Math.min(newTop, 40));
    },
  },
      
  beforeUnmount() {
    // Remove event listener to prevent memory leaks
    window.removeEventListener('scroll', this.handleScroll);
  },
};
</script>

<style scoped>
html, body {
  height: 100%; 
  margin: 0;
  overflow-y: scroll; 
}

.main-background {
  /* overflow-y: scroll; */
  position: relative; 
  height: 200vh; /* Full height */
  overflow: hidden;
}

.second-background {
  display: flex;
  justify-content: center;
  position: absolute;
  bottom: 0; /* Start at the bottom */
  height: 100%;
  width: 100%; /* Full width */
  z-index: 1;
}
.image-container {
  border: red solid;
  position: sticky;
  top: 0;
  }

  .image-container img {
      width: 100%; /* Make the image responsive */
      height: auto; /* Maintain aspect ratio */
  }

  .sticky-div {
            position: sticky; /* Enable sticky positioning */
            bottom: 0; /* Stick to the bottom of the parent */
            background-color: white; /* Background color for visibility */
            padding: 10px; /* Some padding */
            box-shadow: 0 -2px 5px rgba(0, 0, 0, 0.2); /* Optional shadow */
            z-index: 10; /* Ensure it's on top of other elements */
            font-size:40cap;
        }

  .intro{
    border: orange solid;
    height: 10%;
  }
</style> -->

<!-- <template>
  <div class="main">
    <div class="background-container zoomed-in">
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





</style> -->