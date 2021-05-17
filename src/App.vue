<template>
  <div class="container">
    <users-list></users-list>
  </div>
  <div class="container">
    <div class="block" :class="animate"></div>
    <button @click="animateBlock">Animate</button>
  </div>
  <div class="container">
    <!-- transition elements use the v-enter-from -- v-leave to css classes down below -->
    <!-- the name attribute allows a custom css name for the enter and leave classes -->
    <!-- @enter is the equivalent of the active css class -->
    <!-- :css=false binds the prop and tells the element to skip css -->
    <transition 
    :css="false" 
    @before-enter="beforeEnter" 
    @before-leave="beforeLeave" 
    @enter="enter" 
    @after-enter="afterEnter"
    @leave="leave"
    @after-leave="afterLeave"
    @enter-cancelled="enterCancelled"
    @leave-cancelled="leaveCancelled">
    <p v-if="paraisVisible">This is visible</p>
    </transition>
    <button @click="toggleParagraph">Toggle Paragraph</button>
  </div>
  <div class="container">
    <transition name="fade-button" mode="out-in">
      <!-- can use multiple children instead of just one if you use v-if v-else -->
      <button @click="showUsers" v-if="!usersAreVisbile">Show Users</button>
      <button @click="hideUsers" v-else>Hide Users</button>
    </transition>
  </div>
  <base-modal @close="hideDialog" :open="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>
  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>  

<script>
import UsersList from './components/ListData.vue'
export default {
  components: {
    UsersList
  },
  data() {
    return { 
      animatedBlock: false,
      dialogIsVisible: false,
      paraisVisible: false,
      usersAreVisbile: false,
      enterInterval: null,
      leaveInterval: null
      };
  },
  computed: {
    animate(){
      if(this.animatedBlock){
        return 'animate';
      }
      return null;
    }
  },
  methods: {
    //there is one parameter that is automatically added to the transition event representing the html element
    beforeEnter(el){
      console.log(el)
      el.style.opacity=0;
    },
    enterCancelled(){
      clearInterval(this.enterInterval)
    },
    leaveCancelled(){
      clearInterval(this.leaveInterval)
    },
    beforeLeave(el){
      console.log(el)
      el.style.opacity = 1;
    },
    enter(el, done){
      console.log(el);
      let round = 1;
      this.enterInterval = setInterval(()=>{
        el.style.opacity = round * 0.01;
        round ++;
        if(round>100){
          clearInterval(this.enterInterval);
          //done() lets vue know when enter is done and will *then cue afteEnter()
          done();
        }
      },20)
    },
    afterEnter(el){
      console.log(el)
    },
    leave(el, done){
      console.log(el)
      console.log(el);
      let round = 1;
      this.leaveInterval = setInterval(()=>{
        el.style.opacity = 1 - round * 0.01;
        round ++;
        if(round>100){
          clearInterval(this.leaveInterval);
          //done() lets vue know when enter is done and will *then cue afteEnter()
          done();
        }
      },20)
    },
    afterLeave(el){
      console.log(el)
    },
    showUsers(){
      this.usersAreVisbile = true;
    },
    hideUsers(){
      this.usersAreVisbile = false;
    },
    toggleParagraph(){
      this.paraisVisible = !this.paraisVisible
    },
    animateBlock(){
      this.animatedBlock = true;
    },
    showDialog() {
      this.dialogIsVisible = true;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
  /* transition: transform 0.3s */
}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}

.animate {
  /* transform: translateX(-150px); */
  animation: slide-fade 0.3s ease-out forwards;
}

.fade-button-enter-from,
.fade-button-leve-to {
  opacity: 0;
}

.fade-button-enter-active{
  /* watch the property and have css change it by 0.3s */
  transition: opacity 0.3s ease-out;
}

.fade-button-leave-active{
  transition: opactity 0.3s ease-in;
}

.fade-button-enter-to,
.fade-button-leave-from {
  opacity: 1;
}

@keyframes slide-fade{
  0%{
    transform: translateX(0) scale(1);
  }

  50%{
    transform: translateX(-120) scale(1.1);
  }

  100%{
    transform: translate(-150px) scale(1);
  }
}

.para-enter-from{
  opacity: 0;
  transform: translateY(-30px);
}

.para-enter-active{
  transition: all 0.3s ease-out;
}

.para-enter-to{
  opacity: 1;
  transform: translateY(0);
}

.para-leave-from{
  opacity: 1;
  transform: translateY(0);
}

.para-leave-active{
  transition: all 0.3s ease-in;
}

.para-leave-to{
  opacity: 0;
  transform: translateY(30px);
}
</style>