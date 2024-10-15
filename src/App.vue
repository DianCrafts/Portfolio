<template>
<div class="main-background" :style="{ backgroundColor: topBackgroundcolor }">
  <div class="intro" :class="{ 'fade-in': index === 0, 'fade-out': index !== 0 }" :style="{color : topTextColor}" >
    <div  class="welcome">Welcome to My Portfolio!</div>
    <div class="info">
      <p>
        I'm <strong>Dian</strong>, and this is a journey about me—a creative thinker, passionate about <em>[your field]</em>.
        <br /><br />
        Discover my projects, experiences, and the ideas that inspire me.
        <br />
        Let’s explore innovation and creativity together! 🌟
      </p>
    </div>
  </div>
</div>

<div class="sticky-part-container">
    <div class="image-wrapper">
      <img class="img" :src="stickyPartImageSrc" />
      <img
        v-for="(item, i) in icons"
        :key="i"
        :src="getImageSrc(i)"
        :class="getImageClass(i)"
        :style="computedStyle(i)"
        @click="updateIndex(i + 1)"  
      />
    </div>
</div>

  <div class="second-background" :style="{ backgroundColor: bottomBackGroundColor, height: computedHeight , color: bottomTextColor}" @scroll="handleScroll">
    <transition name="fade" mode="out-in">
      <div v-if="showContent" class="desc-container" :key="index">
        <div class="title">{{ currentTitle }}</div>
        <div class="detail">
          <p>{{ currentDesc }}</p>
        </div>
      </div>
    </transition>
  </div>
  <div class="slider-container">
    <label class="switch">
      <input type="checkbox" @change="toggleDarkMode" />
      <span class="slider round"></span>
    </label>
  </div>
</template>

<script>
//import { Color } from 'three';

export default {
  data() {
    return {
      darkColor: "#F0F3F8",
      lightColor: "#140000",
      isDarkMode: false,
      showIntro: false,
      items: [
      {
    title: "Interests and Hobbies",
    desc: `
              Yoga, meditation, and mindfulness 🧘‍♀️
              Watching YouTube 📺
              Enjoying sitcoms 
              Listening to music 🎶
              Hiking in nature ⛰️
              Cooking delicious meals 🍳    `
  },
        {
            title: "Education",
            desc: "Bachelor's Degree in Computer Science from XYZ University, specializing in Software Development. Graduated with honors in 2022."
        },
        {
            title: "Professional Experiences",
            desc: "Software Developer at ABC Corp, where I led the development of web applications using React and Node.js, improving user engagement by 30%."
        },
        {
            title: "Projects",
            desc: "Developed a mobile app for tracking fitness goals, which gained over 10,000 downloads in its first month and received a 4.8 rating on the App Store."
        },
        {
            title: "Contact Info",
            desc: "You can reach me at my email: example@email.com or connect with me on LinkedIn: linkedin.com/in/yourprofile."
        },
      ],
      index: 0,
      currentTitle: "", 
      currentDesc: "", 
      porportion: 0.8,
      showContent: false,
      icons: [
        {
          pic: require('@/assets/personal.png'),
          dark: require('@/assets/personal-dark.png'),
          darkTransition: require('@/assets/personal-transition-dark.png'),
          transitionPic: require('@/assets/personal-bigger.png'),
          position: {
            left: 31,
            transitionLeft: 30,
          },
        },
        {
          pic: require('@/assets/uni.png'),
          dark: require('@/assets/uni-dark.png'),
          darkTransition: require('@/assets/uni-transition-dark.png'),
          transitionPic: require('@/assets/uni-transition.png'),
          position: {
            left: 37,
            transitionLeft: 36,
          },
        },
        {
          pic: require('@/assets/work.png'),
          dark: require('@/assets/work-dark.png'),
          darkTransition: require('@/assets/work-transition-dark.png'),
          transitionPic: require('@/assets/work-transition.png'),
          position: {
            left: 44,
            transitionLeft: 43,
          },
        },
        {
          pic: require('@/assets/projects.png'),
          dark: require('@/assets/project-dark.png'),
          darkTransition: require('@/assets/project-transition-dark.png'),
          transitionPic: require('@/assets/project-transition.png'),
          position: {
            left: 50.5,
            transitionLeft: 49.5,
          },
        },
        {
          pic: require('@/assets/contact.png'),
          dark: require('@/assets/contact-dark.png'),
          darkTransition: require('@/assets/contact-transition-dark.png'),
          transitionPic: require('@/assets/contact-transition.png'),
          position: {
            left: 57.5,
            transitionLeft: 56.5,
          },
        },
      ],
      iconTop: 20,
      iconTopTranstion: 6,
      stickyPartLight: require('@/assets/sticky part-revese.png'),
      stickyPartDark: require('@/assets/sticky part.png'), 
    };
      
  },
  computed: {
    computedHeight() {
      const screenHeight = window.innerHeight;
      const height = this.porportion * screenHeight * this.items.length;
      return `${height}px`;
    },
    stickyPartImageSrc() {
      return this.isDarkMode ?  this.stickyPartLight : this.stickyPartDark;
    },
    topTextColor() {
      if (this.isDarkMode) {
        return this.darkColor;
      } else {
        return this.lightColor;
      } 
    },

    bottomTextColor() {
      if (this.isDarkMode) {
        return this.lightColor;
      } else {
        return this.darkColor;
      } 
    },
    topBackgroundcolor(){
      if (this.isDarkMode) {
        return this.lightColor; 
      } else {
        return this.darkColor;
      }
    },
    bottomBackGroundColor(){
      if (this.isDarkMode) {
       return this.darkColor;
      } else {
       return this.lightColor; // Light mode second background
      }
    }

  },
  watch: {
    index(newValue, oldValue) {
      if (newValue == 0){
        this.showContent = false,
        this.fadeInIntro();
      }
      if (newValue >= 0.5){
        this.fadeOutIntro();
      }
      if (newValue> 0 && oldValue>0 ){
        this.showContent = false; // Hide the content with fade out
        this.updateCurrentItem(newValue); // Update the content after fade out
      }
      if (newValue> 0 && oldValue == 0 ){
        this.updateCurrentItem(newValue);
        this.showContent = true;
        
      }
      
      
    },
    showContent(newValue){
      if (newValue == false && this.index > 0)
        this.showContent = true;
    },
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
    this.updateCurrentItem(this.index); // Initialize content on mount
    this.showIntro = true; // Show intro on mount
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
      }
      if ((scrollTop > ( 0.2* screenHeight))){
        this.index = 0.5
      } 
      var temp = Math.round(scrollTop / (this.porportion * screenHeight));
      if (temp >= 1) {
        this.index = temp
      }
    },
    updateCurrentItem(index) {
      // Update the currentTitle and currentDesc based on the new index
      this.currentTitle = this.items[index - 1]?.title || "";
      this.currentDesc = this.items[index - 1]?.desc || "";
    },

    getImageSrc(i) {
      if (!this.isDarkMode)
        return this.index === i + 1 ? this.icons[i].transitionPic : this.icons[i].pic;
      else
        return this.index === i + 1 ? this.icons[i].darkTransition : this.icons[i].dark;
    },
    computedStyle(i) {
      if (this.index === i + 1) {
        return {
          top: this.iconTopTranstion + '%',
          left: this.icons[i].position.transitionLeft + '%',
        };
      } else {
        return {
          top: this.iconTop + '%',
          left: this.icons[i].position.left + '%',
        };
      }
    },
    getImageClass(i) {
      return this.index === i + 1 ? 'overlay-image-transition' : 'overlay-image';
    },
    updateIndex(newIndex) {
      this.index = newIndex; 
      this.updateScrollPosition();
    },

    updateScrollPosition() {
      const targetScroll = (window.innerHeight * this.index ) * this.porportion ;//this.index * this.proportion * window.innerHeight; // Ensure this corresponds to downward movement
      window.scrollTo({
        top: targetScroll
        //behavior: 'smooth', // Optional: smooth scroll to the target position
      });
    },
    fadeOutIntro() {
      this.showIntro = false;
    },
    fadeInIntro() {
      this.showIntro = true; 
    },
    toggleDarkMode(event) {
      this.isDarkMode = event.target.checked;
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
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap'); /* Importing Poppins font */

.main-background {
  display: flex;
  width: 100%;
  justify-content: center;
  position: relative;
  height: 70vh;
  overflow: hidden;
  background-color: #f0f0f0; /* Soft background color */
}

.intro {
  display: flex;
  justify-content: center;
  width: 70%;
  margin: auto;
  opacity: 1;
  transition: opacity 1.5s ease-in-out; /* Smooth fade effect */
}

.welcome {
  margin-right: 5%;
  font-size: 4vw; /* Responsive font size */
  font-family: 'Poppins', sans-serif; /* Apply Poppins font */
  font-weight: 600; /* Make it bold */
  /* color: #333;  */
  letter-spacing: 1.5px; /* Space out letters slightly */
  animation: slideInLeft 1.2s ease forwards; /* Slide-in effect */
}

.info {
  border-left: 3px solid #555; /* Softer border color */
  padding-left: 20px;
  font-size: 1.5vw;
  font-family: 'Poppins', sans-serif; /* Font for the info text */
  font-weight: 300; /* Light and elegant */
  /* color: #444;  */
  line-height: 1.6; /* Increase line height for readability */
  animation: slideInRight 1.2s ease forwards;
}

p {
  margin: 0;
}

strong {
  font-weight: 600;
}

em {
  font-style: italic;
  color: #0077cc; /* Highlighted color */
}

/* Fade-in and Fade-out classes */
.fade-in {
  opacity: 1;
  transition: opacity 1s ease-in-out;
}

.fade-out {
  opacity: 0;
  transition: opacity 0.8s ease-in-out;
}

/* Animation keyframes */
@keyframes slideInLeft {
  from {
    transform: translateX(-100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

@keyframes slideInRight {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
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
  top: 45%;
  width: 70%;
  z-index: 10;
}
.title {
  margin-right: 5%;
  height: 100%;
  width: 30%;
  font-size: 3.5vw;
  text-align: center;
}
.detail {
  border-left: white solid;
  /* height: 100%; */
  padding-left: 1%;
  font-size: 2vw;
}

.sticky-part-container {
  margin: 0;
  height: fit-content;
  width: 100%;
  top: 0;
  margin-bottom: -10px;
  position: sticky;
  z-index: 10;
  margin-bottom: -10;
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
.overlay-image-transition {
  position: absolute; /* Overlay images will be positioned relative to the image-wrapper */
  width: 4.5%; /* Set your desired size */
  height: 50%;
  transition: all 0.7s ease;
}
/* Fade transition styles */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.4s;
}
.fade-enter-from, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}
.slider-container {
  position: fixed;
  bottom: 2vh; /* 20% from the top of the viewport */
  left: 2vw; /* 5% from the right side of the viewport */
  z-index: 999;
}

/* The switch - now made bigger and more responsive */
.switch {
  position: relative;
  display: inline-block;
  width: 3.5vw; /* 10% of the viewport width */
  height: 2vw; /* 6% of the viewport width for a bigger, relative size */
}

/* Hide default HTML checkbox */
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

/* The slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 2vw; /* Make the slider knob bigger and responsive */
  width: 2vw;
  left: 0vw;
  bottom: 0vw;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

input:checked + .slider {
  background-color: #2196f3;
}

input:focus + .slider {
  box-shadow: 0 0 1vw #2196f3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(2vw); /* Adjusted for the bigger slider */
  -ms-transform: translateX(2vw);
  transform: translateX(2vw);
}

/* Rounded slider */
.slider.round {
  border-radius: 6vw; 
}

.slider.round:before {
  border-radius: 50%;
}

</style>
