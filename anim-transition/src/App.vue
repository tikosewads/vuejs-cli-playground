<template>
  <!-- animating rotue  -->
  <router-view></router-view>

  <div class="container">
    <users-list></users-list>
  </div>
  <div class="container">
    <div class="block" :class="{ animate: isBlockAnimated }"></div>
    <button @click="animateBlock">Animate</button>
  </div>

  <!-- ketika pakai element transition di modal di bawah, maka kelas animate nya akan jatuh kesana -->
  <base-modal @close="hideDialog" :open="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>

  <!-- DUA ELEMEN DALAM SATU ELEMEN TRANSITION? HOW?  -->
  <!-- else if  -->
  <!-- mode out in in out, susunan meng-animasi  -->
  <div class="container">
    <transition name="btn" mode="out-in"
      ><button @click="showUsers" v-if="!isUsersVisible">Show</button>
      <button @click="hideUsers" v-else>Hide</button></transition
    >
  </div>

  <div class="container">
    <!-- vue came to help animation -->
    <!-- transition element hanya boleh punya satu child -->
    <!-- menambahkan css utility classes  -->
    <!-- transition event before-enter blabla -->
    <!-- css prop false membuat css jadi tidak bekejra but better performance -->
    <transition
      name="para"
      :css="false"
      @before-enter="beforeEnter"
      @after-enter="afterEnter"
      @enter="enter"
      @befor-leave="beforeLeave"
      @leave="leave"
      @after-leave="afterLeave"
      @enter-cancelled="enterCancelled"
      @leave-cancelled="leaveCancelled"
      ><p v-if="isParagrafShown">Hello</p></transition
    >
    <button @click="toggleParagraf">Toggle Paragraf</button>
  </div>
  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>

<script>
import UsersList from './components/UsersList.vue';
export default {
  components: { UsersList },
  component: { UsersList },
  data() {
    return {
      isBlockAnimated: false,
      dialogIsVisible: false,
      isParagrafShown: false,
      isUsersVisible: false,
      enterInterval: null,
      leaveInterval: null
    };
  },
  methods: {
    // cancel while active animation occur
    enterCancelled(el) {
      console.log(el);
      clearInterval(this.enterInterval);
    },
    leaveCancelled(el) {
      console.log(el);
      clearInterval(this.leaveInterval);
    },
    // animating with js
    beforeEnter(el) {
      el.style.opacity = 0;
    },

    // second arg that done
    enter(el, done) {
      console.log(el);
      let round = 1;
      this.enterInterval = setInterval(() => {
        el.style.opacity = round * 0.1;
        round++;
        if (round > 10) {
          clearInterval(this.enterInterval);
          // after enter jd dieksekusi setelah method done()
          done();
        }
      }, 20);
    },
    afterEnter(el) {
      console.log(el);
    },
    beforeLeave(el) {
      el.style.opacity = 1;
    },
    leave(el, done) {
      let round = 1;
      this.leaveInterval = setInterval(() => {
        el.style.opacity = 1 - round * 0.1;
        round++;
        if (round > 10) {
          clearInterval(this.leaveInterval);
          // after enter jd dieksekusi setelah method done()
          done();
        }
      }, 20);
    },
    afterLeave(el) {
      console.log(el);
    },
    showUsers() {
      this.isUsersVisible = true;
    },
    hideUsers() {
      this.isUsersVisible = false;
    },
    animateBlock() {
      this.isBlockAnimated = !this.isBlockAnimated;
    },
    showDialog() {
      this.dialogIsVisible = true;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    },
    toggleParagraf() {
      this.isParagrafShown = !this.isParagrafShown;
    }
  }
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
  /* animate here */
  /* transition: transform 0.5s ease-out; */
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

/* Animate Here */
.animate {
  /* transform: translateX(-100px); */
  /* forward = keep at final state */
  animation: slide-fade 0.3s ease-out forwards;
}

/* vue comes help */
/* direname dari v jadi para  */
/* nama css-nya pun bisa dirubah */
/* di element transitionnya like enter-to-class="masuk" */

/* .para-enter-from {
  opacity: 0;
  transform: translateY(-30px);
} */

/* .para-enter-active {
  /* transition: all 0.3s ease-out; */
/* animation: slide-scale 0.3s ease-out; */
/* } */

/* .para-enter-to {
  opacity: 1;
  transform: translateY(0);
} */

/* .para-leave-from {
  opacity: 1;
  transform: translateY(0);
} */

/* .para-leave-active {
  /* transition: all 0.3s ease-in; */
/* animation: slide-scale 0.3s ease-out; */
/* } */
*/

/* .para-leave-to {
  opacity: 0;
  transform: translateY(30px);
} */
/* define animation */

.btn-enter-from,
.btn-leave-to {
  opacity: 0;
}

.btn-enter-active {
  transition: opacity 0.3s ease-out;
}

.btn-leave-active {
  transition: opacity 0.3s ease-in;
}

.btn-enter-to,
.btn-leave-from {
  opacity: 1;
}

@keyframes slide-scale {
  0% {
    transform: translateX(0) scale(1);
  }
  70% {
    transform: translateX(-120px) scale(1.1);
  }
  100% {
    transform: translateX(-150) scale(1);
  }
}
</style>
