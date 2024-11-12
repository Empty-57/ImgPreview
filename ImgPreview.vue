<script setup>
import {onMounted} from "vue";

let img_flag = true;

function imgPreview(e) {
  document.querySelector('#inside_box').style.transform = 'none'
  const img_mask = document.querySelector('#img_mask')
  const window_width = document.documentElement.clientWidth;
  const window_height = document.documentElement.clientHeight;

  function imgClose_() {
    img_flag = true
    img_mask.style.opacity = 0
    img_mask.style.zIndex = -1
    e.target.style.transform = 'none'
    e.target.style.zIndex = 0
    window.removeEventListener('scroll', imgClose_)
  }

  function imgOpen_() {
    img_mask.style.zIndex = 98
    img_mask.style.opacity = 0.8
    const left = (window_width / 2) - (e.target.width / 2) - e.target.getBoundingClientRect().left
    const top = (window_height / 2) - (e.target.height / 2) - e.target.getBoundingClientRect().top
    const scale_ = (window_width - e.target.width) <= (window_height - e.target.height) ? window_width / e.target.width : window_height / e.target.height
    e.target.style.zIndex = 99
    e.target.style.transform = `translate(${left}px,${top}px) scale(${scale_})`
  }

  if (img_flag) {
    img_flag = false
    requestAnimationFrame(imgOpen_)
    window.addEventListener('scroll', imgClose_, {once: true})
  } else {
    requestAnimationFrame(imgClose_)
  }
}

function imgClose(e) {
  img_flag = true
  const img = document.querySelectorAll('img')

  function closeAnimation() {
    e.target.style.opacity = 0
    e.target.style.zIndex = -1
    img.forEach(item => {
      item.style.transform = 'none'
      item.style.zIndex = 0
    })
  }

  window.removeEventListener('scroll', imgClose)
  requestAnimationFrame(closeAnimation)
}

onMounted(() => {
  let count = 0;
  const timer = setInterval(() => {
    const img = document.querySelectorAll("img");
    if (img) {
      count = 0
      document.querySelector('#inside_box').style.transform = 'none'
      try {
        img.forEach((item) => {
          item.style.transition = '300ms'
          item.style.position = 'relative'
          item.style.objectFit = 'cover'
          item.style.maxWidth = '100%'
          item.style.height = '100%'
          item.onclick = imgPreview
        })
      } catch (err) {
        clearInterval(timer)
      }
      clearInterval(timer)
    } else {
      count++;
      if (count === 5) {
        clearInterval(timer);
      }
    }
  }, 500)
})
</script>

<template>
  <div id="img_mask" class="fixed min-h-full w-full bg-zinc-900 left-0 top-0 duration-300" @click="imgClose"></div>
</template>
<style>
#img_mask {
  opacity: 0;
  z-index: -1;
}
</style>
