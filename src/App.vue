<template>
  <router-view></router-view>
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