<script setup>


import { onMounted, onUnmounted } from "vue";
const props = defineProps({
  size: {type: String, default: '30px'},
  showLogo: {type: Boolean, default: true}
})
function goGoogly(e) {
  let pupils = document.querySelectorAll(".pupil");
  let mouseX = 0;
  let mouseY = 0;

  for (let i = 0; i < pupils.length; i++) {
    let offset = pupils[i].getBoundingClientRect();
    pupils[i]['centerX'] = offset.left + offset.width / 2,
        pupils[i]['centerY'] = offset.bottom;
  }

  mouseX = e.clientX;
  mouseY = e.clientY;

  for (let i = 0; i < pupils.length; i++) {
    pupils[i]['radians'] = Math.atan2(mouseX - pupils[i]['centerX'], mouseY - pupils[i]['centerY']),
        pupils[i]['degree'] = (pupils[i]['radians'] * (180 / Math.PI) * -1);
    pupils[i].style.transform = 'rotate(' + (pupils[i]['degree'] + 180) + 'deg)';
  }
}

onMounted(() => {
  window.addEventListener('mousemove', goGoogly);
  window.addEventListener('touchstart', goGoogly);
  window.addEventListener('touchmove', goGoogly);
})

onUnmounted(() => {
  window.removeEventListener('mousemove', goGoogly);
  window.removeEventListener('touchstart', goGoogly);
  window.removeEventListener('touchmove', goGoogly);
})


</script>

<template>

    <div id="face" :style="{'width': size, 'height': size}">
      <div class="left eye">
        <div class="pupil"></div>
      </div>
      <div class="right eye">
        <div class="pupil"></div>
      </div>

      <div class="d-none d-sm-block" style="position:relative;left:40px;top:0px" v-if="showLogo">
        <span style="color:#FFA328;font-weight: 600">PageWatch.ai</span>
      </div>
    </div>

</template>

<style>
div#face {
  width: 40px;
  height: 40px;
  position: relative;
  left: 12px;
  background: #FFA328;
  margin-top: 0rem;
}

div#face, div#face * {
  border-radius: 50%;
}

.sidebar-mobile {
  position: absolute;
  z-index: 999;
}

.sidebar-mobile-close {
  position: absolute;
  right: 20px;
  top: 20px;
}

.eye {
  width: 25%;
  height: 25%;
  background: #fff;
  position: absolute;
  top: 30%;
}

.left.eye {
  left: 20%;
}

.right.eye {
  right: 20%;
}

.pupil {
  background: #8f8f8f;
  width: 50%;
  height: 50%;
  position: relative;
  left: 25%;
  transform-origin: bottom;
  transform: rotate(120deg);
}


.logo {
  margin: 1px -4px;
  font-size: 18px;
  width: 100%;
  text-align: center;

  a {
    padding: 0 5px;
    text-decoration: none;
    white-space: nowrap;
  }
}


</style>