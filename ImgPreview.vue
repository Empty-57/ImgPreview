<script>
let img_flag = true;
export default {
  methods: {
    imgPreview(e) {
      const img_mask = document.querySelector('#img_mask')
      const window_width = document.documentElement.clientWidth;
      const window_height = document.documentElement.clientHeight;

      function imgClose() {
        img_flag = true
        img_mask.style.opacity = 0
        img_mask.style.zIndex = -1
        e.target.style.transform = 'none'
        e.target.style.zIndex = 0
        window.removeEventListener('scroll', imgClose)
      }

      function imgOpen() {
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
        requestAnimationFrame(imgOpen)
        window.addEventListener('scroll', imgClose, {once: true})
      } else {
        requestAnimationFrame(imgClose)
      }
    },
    imgClose(e) {
      img_flag = true
      const img = document.querySelectorAll('img')

      function closeAnimation() {
        e.target.style.opacity = 0
        e.target.style.zIndex = -1
        img.forEach(img_e => {
          img_e.style.transform = 'none'
          img_e.style.zIndex = 0
        })
      }

      requestAnimationFrame(closeAnimation)
    },
  },
  mounted() {
    let count = 0;
    const timer2 = setInterval(() => {
      if (document.querySelector("img")) {
        count = 0
        const img = document.querySelectorAll("img");
        try {
          img.forEach((item) => {
            item.style.transition = '300ms'
            item.style.position = 'relative'
            item.style.objectFit = 'cover'
            item.style.maxWidth = '100%'
            item.style.height = '100%'
            item.onclick = this.imgPreview
          })
        } catch (err) {
          clearInterval(timer2)
        }
        clearInterval(timer2)
      } else {
        count++;
        if (count === 5) {
          clearInterval(timer2);
        }
      }
    }, 500)
}
</script>

<template>
  <div id="img_mask" class="fixed min-h-full w-full bg-zinc-900 left-0 top-0 duration-300" @click="imgClose"></div>
</template>
