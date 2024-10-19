<template> 
  <div class="white-main-contatiner" :style="{height : computedHeight}" @scroll ="handleScroll">
    <div class= "fixed-screen">
       <div class="content-wrapper">
        
         <transition>
               <div class="intro" v-if="showIntro" > 
                        <div  class="welcome" >{{ welcomeShownText }}</div>
                        <div class="info" v-show="isVisible" >
                          <p>I'm <strong>Dian</strong>, and this is a journey about me—a creative thinker, passionate about <em>[your field]</em>.
                          <br /><br />
                          Discover my projects, experiences, and the ideas that inspire me.
                          <br />
                          Let’s explore innovation and creativity together! 🌟</p>     
                        </div>
                        <div class="buttons-wrapper">
                            <button class="flat-button" @click="startJourney">START THE JOURNEY</button>
                            <button class="flat-button" @click="toggleContent">MY CV</button>
                        </div>
              </div> 
          </transition>
          <transition>
              <div class="scrolling-data" v-if="showContent" > 
                    <div class="title">{{ currentTitle }}</div>
                    <div class="detail">
                      <p>{{ currentDesc }}</p>
                    </div>
              </div> 
          </transition>
           <div class ="image-container">
           
                <img class="img" :src="road" />
                    <img 
                    v-for="(item, i) in icons"
                    :key="i"
                    :src="getImageSrc(i)"
                    :class="getImageClass(i)"
                    :style="computedStyle(i)"
                    @click="updateScrollTop(i + 1)"   
                  />
           </div>
          </div> 

          <!-- <div id="arrowAnim"  onclick="console.log('Clicked!')">
              <div class="arrowSliding">
                <div class="arrow" ></div>
              </div>
              <div class="arrowSliding delay1">
                <div class="arrow"></div>
              </div>
              <div class="arrowSliding delay2">
                <div class="arrow"></div>
              </div>
              <div class="arrowSliding delay3">
                <div class="arrow"></div>
              </div>
            </div> -->
    </div> 

        
  </div>

</template>

<script>

export default {
  data() {
    return {
      welcomeFullText: "Welcome to My Portfolio!",
      welcomeShownText: "",
      welcomeIndex: 0,
      isVisible: false,
      // typingSound: new Audio('path/to/typing-sound.mp3'), // Add your sound file
      
      road: require('@/assets/road.png'),
      showIntro: true,
      index: 0,
      currentDesc : "",
      currentTitle: "",
      showContent: false,
      indexFractionLoss: 0,
      indexPorportion: 0.7,
      scrollPorportion: 0.8,  
      
      items: [
      {
      title: "Interests and Hobbies",
      desc: 
                "Yoga, meditation, and mindfulness 🧘‍♀️"

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
      icons: [
        {
          pic: require('@/assets/personal.png'),
          transitionPic: require('@/assets/personal-transition.png'),
          position: {
            left: 4,
            top: 9,
            transitionLeft: 3,
            transtitionTop: 4,
          },
        },
        {
          pic: require('@/assets/uni.png'),
          transitionPic: require('@/assets/uni-transition.png'),
          position: {
            left: 10,
            top: 44,
            transitionLeft: 9,
            transtitionTop: 39
          },
        },
        {
          pic: require('@/assets/work.png'),
          transitionPic: require('@/assets/work-transition.png'),
          position: {
            left: 30,
            top: 68,
            transitionLeft: 28.9,
            transtitionTop: 63
          },
        },
        {
          pic: require('@/assets/projects.png'),
          transitionPic: require('@/assets/projects-transition.png'),
          position: {
            left: 55,
            top: 87.5,
            transitionLeft: 54,
            transtitionTop: 82.5
          },
        },
        {
          pic: require('@/assets/contact.png'),
          transitionPic: require('@/assets/contact-transition.png'),
          position: {
            left: 70,
            top: 75,
            transitionLeft: 69,
            transtitionTop: 70
          },
        },
      ]
    }
  },
  computed: {
    computedHeight() {
      const height = this.items.length * window.innerHeight; 
      return `${height}px`;
    },
    

  },
  watch: {
    index(newValue){
      this.showContent = false;
      this.showIntro = false;
      if (newValue > 0){
        this.updateCurrentItem(newValue)
        setTimeout(() => {
            this.showContent = true;
        }, 200);
      }
      else {
        setTimeout(() => {
            this.showIntro = true;
        }, 200);
        
      }
    }
  },
  mounted() {
    this.type();
    window.addEventListener("scroll", this.handleScroll);
    setTimeout(() => {
            this.isVisible = true;
        }, 1000);
    
  },
  beforeUnmount() {
    window.removeEventListener("scroll", this.handleScroll);
  },
  methods:{
    handleScroll(){
    const scrollTop = document.documentElement.scrollTop;
    const totalScrollableHeight = document.documentElement.scrollHeight - window.innerHeight; // Total scrollable height (content height - window height)
    const scrollPercentage = scrollTop / (totalScrollableHeight * this.indexPorportion);
    var temp = Math.floor(scrollPercentage * this.items.length)
    if (temp <= this.items.length )
    this.index = temp ;
    },
    getImageSrc(i) {
      return this.index === i + 1 ? this.icons[i].transitionPic : this.icons[i].pic;
    },
    getImageClass(i) {
        return this.index === i + 1 ? 'overlay-image-transition' : 'overlay-image';
    },
    computedStyle(i) {
      if (this.index === i + 1) {
        return {
          top: this.icons[i].position.transtitionTop + '%',
          left: this.icons[i].position.transitionLeft + '%',
        };
      } else {
        return {
          top: this.icons[i].position.top + '%',
          left: this.icons[i].position.left + '%',
        };
      }
    },
    updateScrollTop(newIndex) {
      console.log("*********")
      const totalScrollableHeight = document.documentElement.scrollHeight - window.innerHeight; 
      const scrollPercentage = newIndex / this.items.length;
      var scrollTop = (scrollPercentage * totalScrollableHeight)* this.scrollPorportion;
      window.scrollTo({
        top: scrollTop,
      });
    },
    updateCurrentItem(index) {
      this.currentTitle = this.items[index - 1]?.title || "";
      this.currentDesc = this.items[index - 1]?.desc || "";
      
    },
    type() {
      if (this.welcomeIndex < this.welcomeFullText.length) {
        this.welcomeShownText += this.welcomeFullText.charAt(this.welcomeIndex);
        this.welcomeIndex++;
        setTimeout(this.type, 50);
      }
    },
    startJourney(){
      this.updateScrollTop(1)
    },
    

  },

 
   
 

};
</script>


<style>

html,
body {
  height: 100%;
  margin: 0;
}

.white-main-contatiner {
  position: relative;
  width: 100%;
  background-color: white;
  z-index: 10;
}


.fixed-screen{
  top: 0;
  position: sticky;
  height: 95vh;  
  top: 2%;
}
.content-wrapper {
  position: relative;
  height: 100%;
 
}

.scrolling-data{
  position: absolute;
  padding-top: 8%;
  /* display: flex; */
  justify-content: center;
  
  width: 50%;
  right: 20%;
  height: 45%;
}
.title{
  display: flex;
  justify-content: center;
  font-size: 2vw; 
  font-family: 'Poppins', sans-serif; 
  font-weight: 600; 
  letter-spacing: 1.5px;
  /* animation: slideInLeft 1.2s ease forwards;  */
  z-index: 5;
}
.detail{
  display: flex;
  justify-content: center;
  padding-left: 20px;
  font-size: 1vw;
  font-family: 'Poppins', sans-serif; 
  font-weight: 300;
  height: fit-content;
  padding: 10%;
  z-index: 5;
}


.intro {
  position: absolute;
  padding-top: 8%;
  /* display: flex; */
  justify-content: center;
  width: 75%;
  right: 0;
  height: 45%;
}

.welcome {
  /* width: 30%; */
  position: absolute;
  /* left: 0; */
  font-size: 4vw; 
  font-family: 'Poppins', sans-serif; 
  font-weight: 600; 
  letter-spacing: 1.5px; 
  z-index: 5;
}

.info {
  position: absolute;
  margin-top: 10%;
  padding-left: 20px;
  font-size: 1.5vw;
  font-family: 'Poppins', sans-serif; 
  font-weight: 300;
  height: fit-content;
  z-index: 5;
}

.buttons-wrapper{
  height: 20%;
  bottom: 10%;
  position: absolute;
  width: 20%;
  right: 15%;
}
.flat-button{
  position: relative;
  width: 100%;
  margin: 1%;
  height: 40%;
  z-index: 10;
  background: none;
  border: 2px solid black; /* Border with flat color */
  color: black; /* Flat blue text */
  padding: 10px 20px;
  font-size: 1vw;
  border-radius: 4px; /* Slight rounding for a clean look */
  cursor: pointer;
  animation: float 3s ease-in-out infinite;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.flat-button:hover {
  background-color: #EA8A6F;
  font-size: 1.1vw;
  animation: none; 
}

p {
  margin: 0;
}

strong {
  font-weight: 600;
}

em {
  font-style: italic;
  color: #0077cc;
}  

.image-container{
  margin: auto;
  height: 100%;
  width: 100%;
  bottom: 0;
  position: absolute;
  z-index: 2;
}

.img {
  width: 100%;
  height: 85%;
  bottom: 0;
  position: absolute;
  clip-path: inset(0 100% 0 0);
  animation: reveal 1.5s ease-in-out forwards;
}

.overlay-image {
  position: absolute; 
  width: 4%; 
  height: 12%;
  transition: all 0.7s ease;
}
.overlay-image-transition {
  position: absolute; 
  width: 6%;
  height: 17%;
  transition: all 0.7s ease;
}
@keyframes reveal {
  0% {
    clip-path: inset(0 100% 0 0); /* Image fully hidden */
  }
  100% {
    clip-path: inset(0 0 0 0); /* Image fully revealed */
  }
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0); /* Start and end at the original position */
  }
  50% {
    transform: translateY(-20px); /* Move up by 10px */
  }
}




/* #arrowAnim {
  background-color: red;
  transform: rotate(90deg);
  bottom: 10%;
  left:2%;

  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
}

.arrow {
  width: 0.5vw;
  height: 1vh;
  border: 0.2vw solid;
  border-color: black transparent transparent black;
  transform: rotate(-45deg);
}


.arrowSliding {
  position: absolute;
  -webkit-animation: slide 4s linear infinite; 
          animation: slide 4s linear infinite;
}

.delay1 {
  -webkit-animation-delay: 1s; 
    animation-delay: 1s;
}
.delay2 {
  -webkit-animation-delay: 2s; 
    animation-delay: 2s;
}
.delay3 {
  -webkit-animation-delay: 3s; 
    animation-delay: 3s;
}

@-webkit-keyframes slide {
    0% { opacity:0; transform: translateX(3vw); }	
   20% { opacity:1; transform: translateX(1vw); }	
   80% { opacity:1; transform: translateX(-1vw); }	
  100% { opacity:0; transform: translateX(-3vw); }	
}
@keyframes slide {
    0% { opacity:0; transform: translateX(3vw); }	
   20% { opacity:1; transform: translateX(1vw); }	
   80% { opacity:1; transform: translateX(-1vw); }	
  100% { opacity:0; transform: translateX(-3vw); }	
} */




</style>



