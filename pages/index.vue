<template>
  <main class="max-w-6xl w-full justify-self-center">
    <template v-for="(slide, index) in slides">
      <Slide v-if="index === selectedSlideIndex" :key="index">
        <div
          class="absolute right-0 top-0 bg-white p-2 w-10 h-10 -mt-4 mr-2 sm:-mr-4 sm:mt-2 border-2 text-center border-green-600 rounded text-green-600"
        >
          {{ adjustedSlideIndex }}
        </div>
        <nuxt-content :document="slide" />
      </Slide>
    </template>
    <div class="flex justify-center mt-4">
      <p>
        Use <kbd>&#8592;</kbd>, <kbd>&#8594;</kbd>, or <kbd>enter</kbd> to
        navigate slides.
      </p>
    </div>
  </main>
</template>

<script>
export default {
  async asyncData({ $content }) {
    const slides = await $content('slides').sortBy('order').fetch()
    return {
      slides,
    }
  },
  data() {
    return {
      selectedSlideIndex: 0,
    }
  },
  computed: {
    totalSlides() {
      return this.slides.length
    },
    adjustedSlideIndex() {
      return this.selectedSlideIndex + 1
    },
  },
  mounted() {
    window.addEventListener('keydown', (KeyboardEvent) => {
      this.handleGlobalKeyDown(KeyboardEvent)
    })
  },
  methods: {
    handleGlobalKeyDown(KeyboardEvent) {
      switch (KeyboardEvent.keyCode) {
        case 37:
          this.prev()
          break
        case 39:
          this.next()
          break
        case 13:
          this.next()
          break
      }
    },
    next() {
      if (this.totalSlides === this.adjustedSlideIndex) return
      this.selectedSlideIndex++
    },
    prev() {
      if (this.selectedSlideIndex === 0) return
      this.selectedSlideIndex--
    },
  },
}
</script>
