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
          <img
            v-for="(item, i) in icons"
            :key="i"
            :src="getImageSrc(i)"   
            :class="getImageClass(i)"
            :style="computedStyle(i)"
          />    
    </div>
  </div>

  <div class="second-background" :style="{ backgroundColor: secondBackgroundColor, height: computedHeight }" @scroll="handleScroll">
    <transition name="fade" mode="out-in">
      <div v-if="showContent" class="desc-container" :key="index">
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
        { title: "Item 4", desc: "This is the description for item 4." },
      ],
      index: 0,
      currentTitle: "", // Title of the current item
      currentDesc: "", // Description of the current item
      porportion: 0.8,
      showContent: false,
      icons: [
        {
          pic: require('@/assets/uni.png'),
          transitionPic: require('@/assets/uni-transition.png'),
          position: {
            left: 37,
            transitionLeft: 36,
          },
        },
        {
          pic: require('@/assets/work.png'),
          transitionPic: require('@/assets/work-transition.png'),
          position: {
            left: 44,
            transitionLeft: 43,
          },
        },
        {
          pic: require('@/assets/projects.png'),
          transitionPic: require('@/assets/project-transition.png'),
          position: {
            left: 50.5,
            transitionLeft: 49.5,
          },
        },
        {
          pic: require('@/assets/contact.png'),
          transitionPic: require('@/assets/contact-transition.png'),
          position: {
            left: 57.5,
            transitionLeft: 56.5,
          },
        },
      ],
      iconTop: 20, // Fixed top position
      iconTopTranstion: 8, // Transition top position if needed
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
    index(newValue, oldValue) {
      if (newValue> 0 && oldValue>0 ){
        this.showContent = false; // Hide the content with fade out
        this.updateCurrentItem(newValue); // Update the content after fade out
      }
      if (newValue> 0 && oldValue == 0 ){
        this.updateCurrentItem(newValue);
        this.showContent = true;
      }
      if (newValue == 0)
        this.showContent = false
      
    },
    showContent(newValue){
      if (newValue == false && this.index > 0)
        this.showContent = true;
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
    },
    updateCurrentItem(index) {
      // Update the currentTitle and currentDesc based on the new index
      this.currentTitle = this.items[index - 1]?.title || "";
      this.currentDesc = this.items[index - 1]?.desc || "";
    },

    getImageSrc(i) {
      console.log("************" + i)
      var result =this.index === i + 1 ? this.icons[i].transitionPic : this.icons[i].pic;
      
      return result
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


</style>
