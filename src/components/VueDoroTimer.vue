<!--Aqui precisei instalar vue-radial-progress, para fazer o progresso do contador,
conforme o tempo for avançando -->
<template>
  <radial-progress-bar
    :diameter="timerDiameter"
    :completed-steps="completedSteps"
    :total-steps="timerTotalSteps"
    :stroke-width="5"
    :inner-stroke-width="2"
    inner-stroke-color="#888"
    start-color="#888"
    stop-color="#888"
    :is-clockwise="false"
>
  <span class="timer__numbers">{{ actualTimer }}</span>
  </radial-progress-bar>
</template>

<script>
import RadialProgressBar from 'vue-radial-progress'
import moment from 'moment'
import alarm from '@/assets/alarm.mp3'

export default {
  name: 'VueDoroTimer',
  components: {
    RadialProgressBar,
  },
  props: {
    // aqui vamos cadastrar as props que estamos recebendo lá no VueDoro//
    timer: {
      type: String,
      required: true,
    },
    isPlaying: {
      // o vue não aceita que eu escreva aqui na props dessa forma=>
      // is-playing, por isso tive que passar desta forma
      // mas este is Playing é o is-playing lá do VueDoro
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      actualTimer: '',
      timerInterval: null,
      timerDiameter: 400,
    }
  },
  computed: {
    timerTotalSteps() {
      return this.transformTimeToNumber(this.timer)
    },
    completedSteps() {
      return this.transformTimeToNumber(this.actualTimer) - Math.abs(this.timerTotalSteps)
    },
  },
  mounted() {
    this.setTimerWidth()
    window.addEventListener('resize', this.setTimerWidth)
    this.setActualTimer(this.timer)
  },
  watch: {
    timer(value) {
      this.setActualTimer(value)
      if (this.isPlaying) {
        this.stopCountDownTimer()
        this.initCountDownTimer()
      }
    },
    isPlaying(playing) {
      if (playing) {
        this.initCountDownTimer()
      } else {
        this.stopCountDownTimer()
        this.changePageTitle('VueDoro')
      }
    },
    actualTimer(value, oldValue) {
      if (this.isPlaying) {
        this.changePageTitle(value)
      }
      if (oldValue === '00:01' && value === '00:00') {
        const LAST_SECOND = 1000
        setTimeout(() => {
          this.stopCountDownTimer()
          this.playAlarm()
          this.setTimeFinished()
        }, LAST_SECOND)
      }
    },
  },
  methods: {
    transformTimeToNumber(time) {
      return +time.replace(':', '')
    },
    setActualTimer(timer) {
      this.actualTimer = timer
    },
    initCountDownTimer() {
      const INTERVAL_TIME = 1000
      this.timerInterval = setInterval(() => {
        this.subtractOneSecond()
      }, INTERVAL_TIME)
    },
    subtractOneSecond() {
    // aqui instalei outra biblioteca => npm i moment //
      const subtractedTime = moment(this.actualTimer, 'mm:ss')
        .subtract(1, 'seconds')
        .format('mm:ss')
      this.setActualTimer(subtractedTime)
    },
    stopCountDownTimer() {
      clearInterval(this.timerInterval)
    },
    setTimeFinished() {
      this.$emit('timeFinished')
    },
    playAlarm() {
      const audio = new Audio(alarm)
      audio.play()
    },
    changePageTitle(value) {
      document.title = value
    },
    setTimerWidth() {
      const { width } = window.screen
      if (width <= 450) this.timerDiameter = 300
      if (width < 350) this.timerDiameter = 250
      if (width > 450) this.timerDiameter = 400
    },
  },
}
</script>

<style lang="stylus" scoped>
.timer__numbers
    font-size 6rem
    @media screen and (max-width $mobile)
      font-size 4rem

</style>
