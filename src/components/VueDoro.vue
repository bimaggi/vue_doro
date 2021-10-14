<template>
  <div :class="actualStepClass">
    <app-header/>
    <main class="page__main">
      <h2 class="main__step-title">{{ actualStep.title }}</h2>
      <div class="main__timer">
        {{ actualStep.time }}
      </div>
      <span class="main__rounds">{{actualRound}}/{{rounds}} de hora </span>
      <button @click="setActualStep(steps.shortBreak)">Trocar</button>
    </main>
  </div>
</template>

<script>
import AppHeader from '@/components/AppHeader'
import timer from '@/enums/timer'

const {
  ROUNDS,
  FOCUS,
  SHORT,
  LONG,
} = timer

export default {
  name: 'VueDoro',
  components: {
    AppHeader,
  },
  data() {
    return {
      actualRound: 1,
      rounds: ROUNDS,
      actualStep: null,
      steps: {
        focus: {
          title: FOCUS.LABEL,
          time: FOCUS.TIME,
        },
        shortBreak: {
          title: SHORT.LABEL,
          time: SHORT.TIME,
        },
        longBrake: {
          title: LONG.LABEL,
          time: LONG.TIME,
        },
      },
    }
  },
  created() {
    this.setActualStep(this.steps.focus)
  },
  computed: {
    actualStepClass() {
      const step = {
        [FOCUS.LABEL]: 'focus',
        [SHORT.LABEL]: 'short',
        [LONG.LABEL]: 'long',
      }
      return `page--${step[this.actualStep.title]}`
    },
  },
  methods: {
    setActualStep(value) {
      this.actualStep = value
    },
  },
}
</script>
<style lang="stylus">
pageStatusGradient(endColor)
  background linear-gradient(180deg, #fff -8%, rgba(#fff, 0) 100%),endColor

.page
  display grid
  grid-row-gap 0.9rem
  grid-template-rows 6rem 1fr
  height 100vh
  padding 0 1.3rem

.page--focus
  @extends .page
  pageStatusGradient($cosmos)

.page--short
  @extends .page
  pageStatusGradient($pattens-blue)

.page--long
  @extends .page
  pageStatusGradient($tana)

.page__main
  display grid
  grid-row-gap 2rem
  grid-template-rows 4rem 50% 3rem 6rem
  justify-items center
  align-items center

.main__step-title
  font-size 2.1rem
  font-weight 400
  @media screen and (max-width $mobile)
    font-size 1.7rem

.main__timer
  font-size 6rem

.main__rounds
  letter-spacing .1rem
</style>
