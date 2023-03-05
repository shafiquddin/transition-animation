<template>
  <div class="container">
    <base-user></base-user>
  </div>
  <div class="container">
    <div class="block" :class="{ animation: animate }"></div>
    <button @click="getAnimate">Animate</button>
  </div>
  <div class="container">
    <transition
      :css="false"
      @before-enter="beforeEnter"
      @enter="enter"
      @after-enter="afterEnter"
      @before-leave="beforeLeave"
      @leave="leave"
      @after-leave="afterLeave"
      @enter-cancelled="enterCancelled"
      @leave-cancelled="leaveCancelled"
    >
      <p v-if="paraVisiblity">This paragaph will animate</p>
    </transition>
    <button @click="getToggle">Toggle</button>
  </div>
  <div class="container">
    <transition name="showHide" mode="out-in">
      <button @click="btnVisiblity = true" v-if="!btnVisiblity">Show</button>
      <button @click="btnVisiblity = false" v-else>Hide</button>
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
import BaseUser from './components/BaseUser.vue';
export default {
  data() {
    return {
      dialogIsVisible: false,
      animate: false,
      paraVisiblity: false,
      btnVisiblity: false,
      enterIntervel:null,
      leaveIntervel:null
    };
  },
  components:{
    BaseUser,
  },
  methods: {
    beforeEnter(el){
      console.log(el);
      el.style.opacity=0;
    },
    enter(el,done){
      console.log(el);
      let round=1;
      this.enterIntervel=setInterval(()=>{
        el.style.opacity=round*0.01;
        round++;
        if(round>100){
          clearInterval(this.enterIntervel);
          done();
        }
      },20)
    },
    afterEnter(el){
      console.log(el);
      el.style.opacity=1;
    },
    beforeLeave(el){
      console.log(el);
      el.style.opacity=1;
    },
     leave(el,done){
      console.log(el);
      let round=1;
      this.leaveIntervel=setInterval(()=>{
        el.style.opacity=1-round*0.01;
        round++;
        if(round>100){
          clearInterval(this.leaveIntervel);
          done();
        }
      },20)
    },
    afterLeave(el){
      console.log(el);
      el.style.opacity=0;
    },
    enterCancelled(el){
      console.log(el);
      clearInterval(this.enterIntervel);
    },
    leaveCancelled(el){
      console.log(el);
      clearInterval(this.leaveIntervel);
    },
    getToggle() {
      this.paraVisiblity = !this.paraVisiblity;
    },
    getAnimate() {
      this.animate = true;
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
  /* transition: transform 0.3s ease-out; */
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

.animation {
  /* transform:translateX(-150px); */
  animation: slide-scale 0.3s ease-out forwards;
}

/* .para-enter-from {
  opacity: 0;
  transform: translateY(-30px);
}
.para-enter-active {
  transition: all 2s ease-out;
}
.para-enter-to {
  opacity: 1;
  transform: translateY(0);
}
.para-leave-from {
  opacity: 1;
  transform: translateY(0);
}
.para-leave-active {
  transition: all 0.3s ease-out;
}
.para-leave-to {
  opacity: 0;
  transform: translateY(30px);
} */

.showHide-enter-to,
.showHide-leave-from {
  opacity: 1;
}
.showHide-enter-active {
  transition: opacity 0.3s ease-out;
}
.showHide-leave-active {
  transition: opacity 0.3s ease-in;
}
.showHide-enter-from,
.showHide-leave-to {
  opacity: 0;
}

@keyframes slide-scale {
  0% {
    transform: translateX(0) scale(1);
  }
  70% {
    transform: translateX(-120px) scale(1.1);
  }
  100% {
    transform: translateX(-150px) scale(1);
  }
}
</style>