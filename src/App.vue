<template>
  <div class="main-background" :style="{ backgroundColor: mainBackgroundcolor }">
    <div class="intro">
      <div class="welcome">Welcome!</div>
      <div class="info">
        <p>
          here you should see short info about me <br />
          for instance the face that I am beautiful<br />
          and also cute and smart ^^
        </p>
      </div>
    </div>
  </div>
  <div class="sticky-part-container">
    <div class="image-wrapper">
      <img class="img" src="./assets/sticky part.png" />
      <img class="overlay-image" src="./assets/uni.png" style="top: 20%; left: 37%" />
      <img class="overlay-image" src="./assets/work.png" style="top: 20%; left: 44%" />
      <img class="overlay-image" src="./assets/projects.png" style="top: 20%; left: 50.5%" />
      <img class="overlay-image" src="./assets/contact.png" style="top: 20%; left: 57.5%" />
    </div>
  </div>

  <div class="second-background" :style="{ backgroundColor: secondBackgroundColor, height: computedHeight }" @scroll="handleScroll">
      <div v-if="showContent" class="desc-container" :key="index">
        <div class="title">{{ currentTitle }}</div>
        <div class="detail">
          <p>{{ currentDesc }}</p>
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
      items: [
        { title: "Item 1", desc: "This is the description for item 1." },
        { title: "Item 2", desc: "This is the description for item 2." },
        { title: "Item 3", desc: "This is the description for item 3." },
      ],
      index: 0,
      showContent: false,
      currentTitle: "", // Title of the current item
      currentDesc: "", // Description of the current item
      porportion: 0.8
    };
  },
  computed: {
    computedHeight() {
      const screenHeight = window.innerHeight;
      const height = this.porportion * screenHeight * this.items.length;
      return `${height}px`;
    },
  },
  watch: {
    index(newValue) {
      if (newValue === 0) {
        this.showContent = false;
      } else {
        this.updateCurrentItem(newValue);
      }

      if (newValue > 0) {
          this.showContent = true;
        }
    },
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
    this.updateCurrentItem(this.index); // Initialize content on mount
  },
  beforeUnmount() {
    window.removeEventListener("scroll", this.handleScroll);
  },
  methods: {
    handleScroll() {
      const scrollTop = window.scrollY || document.documentElement.scrollTop;
      const screenHeight = window.innerHeight;
      if (scrollTop < this.porportion * screenHeight) {
        this.index = 0;
      } else {
        this.index = Math.round(scrollTop / (this.porportion * screenHeight));
      }
      console.log(this.index)
    },
    updateCurrentItem(index) {
      // Use index - 1 because index starts at 1 after scrolling
      this.currentTitle = this.items[index - 1]?.title || "";
      this.currentDesc = this.items[index - 1]?.desc || "";
    },
    
  },
};
</script>

<style scoped>
html,
body {
  height: 100%;
  margin: 0;
  overflow-y: scroll;
}

.main-background {
  display: flex;
  width: 100%;
  justify-content: center;
  position: relative;
  height: 70vh;
}
.intro {
  display: flex;
  justify-content: center;
  width: 70%;
  margin: auto;
}

.welcome {
  margin-right: 5%;
  font-size: 10dvh;
}
.info {
  border-left: black solid;
  font-size: 5dvh;
}

.second-background {
  display: flex;
  justify-content: center;
  position: relative;
  width: 100%;
  padding-top: 20%;
}

.desc-container {
  display: flex;
  height: fit-content;
  position: fixed;
  top: 50%;
  width: fit-content;
  z-index: 10;
}
.title {
  margin-right: 5%;
  color: white;
  height: 3%;
  font-size: 10rem;
}
.detail {
  border-left: white solid;
  color: white;
  height: 5%;
  font-size: 5rem;
}

.sticky-part-container {
  margin: 0;
  height: fit-content;
  width: 100%;
  top: 0;
  margin-bottom: -10px;
  position: sticky;
  z-index: 10;
}
.image-wrapper {
  position: relative; /* Make this container relative */
}
.img {
  width: 100%;
}

.overlay-image {
  position: absolute; /* Overlay images will be positioned relative to the image-wrapper */
  width: 3%; /* Set your desired size */
  height: 35%;
}


</style>


<!-- <template>
  
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
  <div class ="sticky-part-container">
    <div class="image-wrapper">
      <img class="img" src="./assets/sticky part.png">
      <img class="overlay-image" src="./assets/uni.png" style="top: 20%; left: 37%;">
      <img class="overlay-image" src="./assets/work.png" style="top: 20%; left: 44%;">
      <img class="overlay-image" src="./assets/projects.png" style="top: 20%; left: 50.5%;">
      <img class="overlay-image" src="./assets/contact.png" style="top: 20%; left: 57.5%;">
    </div>
        
  </div>

  <div class="second-background" :style="{ backgroundColor: secondBackgroundColor, height: computedHeight}" @scroll="handleScroll">
    <transition name="fade" mode="out-in">
      <div v-show="showContent" class = "desc-container">
        <div class="title">{{ currentTitle }}</div>
        <div class="detail">
          <p>{{ currentDesc }}</p>
        </div>
      </div>
    </transition>
  </div>

   
  
  
</template>


<script>
export default {
  data() {
    return {
      mainBackgroundcolor: "#F0F3F8",
      secondBackgroundColor: "#0F172A",
      items: [
        { title: "Item 1", desc: "This is the description for item 1." },
        { title: "Item 2", desc: "This is the description for item 2." },
        { title: "Item 3", desc: "This is the description for item 3." },
        // Add more items as needed
      ],
      index: 0,
      showContent: false,
      currentTitle: "", // Title of the current item
      currentDesc: "", // Description of the current item
      porportion: 0.8
    };
  },
  computed: {
    computedHeight() {
        const screenHeight = window.innerHeight;
        const height = this.porportion * screenHeight * this.items.length ;
        console.log(height)
        return `${height}px`;
    },
  },

  watch: {
    // Watch for changes in the 'index' data property
    index(newValue) {
      if (newValue == 0) {
        this.showContent = false
      }
      // Update the title and description when index changes
      this.updateCurrentItem(newValue);
      if (newValue > 0) {
        this.showContent = true
      }
    },
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
      const scrollTop = window.scrollY || document.documentElement.scrollTop;
      const screenHeight = window.innerHeight;
      if (scrollTop < this.porportion * screenHeight)
        this.index = 0
      else this.index = Math.round(scrollTop/( this.porportion * screenHeight))
      
    },
    updateCurrentItem(index) {
      // Update the currentTitle and currentDesc based on the new index
      this.currentTitle = this.items[index-1]?.title || "";
      this.currentDesc = this.items[index-1]?.desc || "";
    },
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
  /* height: 100vh; */
  width: 100%;  
  /* overflow: visible; */
  padding-top: 20%;
  
}



.desc-container{
  /* margin-right: 10%; */
  display: flex;
  height: fit-content;
  position: fixed; 
  top:50%;          
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

.sticky-part-container{
  margin : 0;
  height: fit-content;
  width: 100%;
  top: 0;
  margin-bottom: -10px;
  position: sticky;
  z-index: 10;
}
.image-wrapper {
  position: relative; /* Make this container relative */
}
.img{
  width: 100%;
}

.overlay-image {
  position: absolute; /* Overlay images will be positioned relative to the image-wrapper */
  width: 3%; /* Set your desired size */
  height: 35%;
}
</style> -->
