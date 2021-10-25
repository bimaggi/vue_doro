<template>
  <div :class="actualStepClass">
    <app-header/>
    <main class="page__main">
      <h2 class="main__step-title">{{ actualStep.title }}</h2>
      <vue-doro-timer
        :timer="actualStep.time"
        :is-playing="isPlaying"
        @timeFinished="setNextStep"
      />
      <span class="main__rounds">{{ actualRound }}/{{ rounds }} rounds </span>
      <vue-doro-actions
        @skipStep="setNextStep"
        @toogleIsPlaying="setIsPlaying"
        @setAdjustments="setAdjustments"
      />
    </main>
  </div>
</template>

<script>
import AppHeader from '@/components/AppHeader'
import VueDoroActions from '@/components/VueDoroActions'
import VueDoroTimer from '@/components/VueDoroTimer'
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
    VueDoroActions,
    VueDoroTimer,
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
        longBreak: {
          title: LONG.LABEL,
          time: LONG.TIME,
        },
      },
      isPlaying: false,
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
    setNextStep() {
      const { title } = this.actualStep
      if (title === LONG.LABEL) {
        this.setActualRound(1)
        this.setActualStep(this.steps.focus)
      } else if (title === FOCUS.LABEL && this.actualRound === this.rounds) {
        this.setActualStep(this.steps.longBreak)
      } else if (title === FOCUS.LABEL && this.actualRound !== this.rounds) {
        this.setActualStep(this.steps.shortBreak)
      } else if (title === SHORT.LABEL && this.actualRound !== this.rounds) {
        this.setActualStep(this.steps.focus)
        this.setActualRound(this.actualRound + 1)
      }
    },
    setIsPlaying(value) {
      // esse value é o valor que estamos passando no segundo parametro do emit
      this.isPlaying = value
    },
    setActualRound(value) {
      this.actualRound = value
    },
    setAdjustments(adjustments) {
      console.log(adjustments)
      // dessa forma transformamos o objeto num array
      Object.entries(adjustments).forEach(([key, value]) => {
        if (key === 'rounds') {
          this.rounds = +value
        } else {
          this.steps[key].time = value
        }
      })
      this.setActualRound(1)
      this.setActualStep(this.steps.focus)
    },
  },
}
</script>
<style lang="stylus">
  .page
    display grid
    grid-row-gap 0.9rem
    grid-template-rows 6rem 1fr
    height 100vh
    padding 0 1.3rem

  .page--focus
    @extends .page
    pageStatusGradient($BlanchedAlmond)

  .page--short
    @extends .page
    pageStatusGradient($lavanda)

  .page--long
    @extends .page
    pageStatusGradient($LightCyan)

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

  .main__rounds
    letter-spacing .1rem
</style>
