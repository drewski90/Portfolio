<template>

  <div 
  @click="newNoise"
  class="star-header-container">

    <canvas ref="starHeader" class="star-header">
    </canvas>

    <v-row no-gutters align-content="center" justify="center" style="width:100%; height: 100%; position:absolute; top:0px; left:0px;">
      <v-col cols="12">
        <div class="star-header-text">ANDREW MARTINEZ</div>
        <div class="star-subheader-text">Full Stack Developer</div>
        <div class="header-contact-links">
          <v-btn icon="mdi-github" href="https://github.com/drewski90" class="mx-2"></v-btn>
          <v-btn icon="mdi-linkedin" href="https://www.linkedin.com/in/andrew-martinez-0179ab51" class="mx-2"></v-btn>
          <v-btn icon="mdi-email" href="mailto: martinezdynamics@gmail.com" class="mx-2"></v-btn>
        </div>
      </v-col>
    </v-row>

  </div>

</template>

<script>

import { createNoise2D } from 'simplex-noise';

function Particle(canvas) {
  let showColor = false
  let Xweight = .002
  let Yweight = .002
  let frame = 0
  let xVel = 1
  let r = 1
  let x, y
  const hue = Math.random() * 360
  function reset() {
    frame = 0
    x = (canvas.width * 1.5) * Math.random() - (canvas.width * .5)
    y = canvas.height * Math.random()
  }
  function updatePosition(simplex) {
    x += xVel
    y += simplex(x * Xweight, y * Yweight)
  }
  function draw(ctx, simplex, dark) {
    if (x > ctx.canvas.width)
      reset(ctx.canvas)
    updatePosition(simplex)
    ctx.beginPath();
    ctx.arc(x, y, r, 0, 2 * Math.PI);
    let color = dark? 
      `hsla(${hue}, ${showColor?'100': '0'}%, 50%, ${frame * .01})`: 
      `hsla(20, 30%, 50%, ${frame * .01})`
    ctx.fillStyle = color
    ctx.fill()
    frame ++
  }
  this.draw = draw
  reset()
}

export default {
  mounted() {
    this.ctx = this.$refs.starHeader.getContext('2d')
    this.init()
    this.changeInterval = setInterval(this.newNoise, 60 * 1000)
    window.addEventListener('resize', this.init)
    this.draw()
    this.observer = new window.IntersectionObserver(
      ([entry]) => this.inView = entry.isIntersecting,
      {rootMargin: "0px", threshold: .1}
    )
    this.observer.observe(this.$refs.starHeader)
  },
  beforeUnmount() {
    this.ctx = null
    clearInterval(this.changeInterval)
    window.removeEventListener('resize', this.init)
  },
  data() {
    return {
      inView:true,
      observer:null,
      changeInterval: null,
      ctx: null,
      nextFrame: null,
      particles: [],
      simplex: createNoise2D(),
    }
  },
  computed: {
    themeName() {
      return this.$vuetify.theme.name
    }
  },
  methods: {
    newNoise() {
      this.simplex = createNoise2D()
    },
    init() {
      const cv = this.$refs.starHeader
      const container = cv.parentNode
      const w = container.clientWidth
      const h = container.clientHeight
      cv.width = w
      cv.height = h
      this.ctx.fillStyle = this.themeName === 'light' ? "hsla(50, 0%, 30%, 1)": "black"
      this.ctx.fillRect(0,0,w,h)
    },
    draw() {
      if (!this.ctx) return
      if (this.inView) {
        const ctx = this.ctx
        const cv = this.$refs.starHeader
        const maxParticles = cv.width 
        ctx.fillStyle = this.themeName === 'light' ?"hsla(50, 0%, 30%, .05)": "rgba(0,0,0,1)"
        ctx.fillRect(0, 0,cv.width, cv.height)
        this.particles.forEach(i=>i.draw(ctx, this.simplex, this.themeName === 'dark'))
        while (this.particles.length < maxParticles)
          this.particles.push(new Particle(ctx.canvas))
        while (this.particles.length > maxParticles)
          this.particles.pop()
      }  
      window.requestAnimationFrame(this.draw)
    },
  }
}

</script>


<style>

.header-contact-links {
  margin-top: 20px;
}

.star-header-container {
  width: 100%;
  height: 90vh !important;
  position:relative;
  padding:0px;
  margin:0px;
  user-select: none;
}

.star-header {
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
}

.star-header-text {
  text-align: center;
  font-family: 'Bayon', sans-serif;
  padding-left: 20px;
  padding-right: 20px;
  font-weight: 100;
  font-size: 6.5em;
  color: rgb(255, 255, 255);
  text-shadow: 0px 0px 5px rgba(0, 0, 0, .8);
}

.star-subheader-text {
  text-align: center;
  font-family: 'Bayon', sans-serif;
  padding-left: 20px;
  padding-right: 20px;
  font-weight: 100;
  font-size: 1em;
  color: rgb(255, 255, 255);
  text-shadow: 0px 0px 5px rgba(0, 0, 0, .8);
}

.star-header-sub-title {
  color: rgb(255, 255, 255);
  text-shadow: 0px 0px 2px rgba(0, 0, 0, 0.8);
}

@media only screen and (min-width: 600px) {
  .star-header-text {
    font-size: 6.5em;
  }
}

@media only screen and (max-width: 600px) {
  .star-header-text {
    font-size: 3.5em;
  }
}


</style>