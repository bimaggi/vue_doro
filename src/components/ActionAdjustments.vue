<template>
    <transition name="modal--fade">
      <div
        class="modal__backdrop"
        @click.self="close"
      >
      <article
        class="modal__container"
        role="dialog"
        aria-labelledby="modalTitle"
        aria-describedby="modalDescrition"
      >
      <header>
        <h1 class="modal__title">Ajustar</h1>
        <button
          class="modal__close"
          @click="close"
        >
          <x-icon size="2x"/>
        </button>
      </header>
      <main class="modal__content">
        <form>
          <div class="content__form">
            <div class="form__input">
              <label for="" class="input__label">Tempo focado</label>
              <input class="input__field" type="text">
            </div>
            <div class="form__input">
              <label for="" class="input__label">Pequena pausa</label>
              <input class="input__field" type="text">
            </div>
            <div class="form__input">
              <label for="" class="input__label">Longa pausa</label>
              <input class="input__field" type="text">
            </div>
            <div class="form__input">
              <label for="" class="input__label">Nº de pausas</label>
              <input class="input__field" type="text">
            </div>
          </div>
          <div class="content__footer">
            <button
              type="submit"
              class="footer__confirm"
            >
              Confirmar
            </button>
          </div>
        </form>
      </main>
      </article>
      </div>
    </transition>
</template>

<script>
import {
  XIcon,
} from 'vue-feather-icons'

export default {
  name: 'ActionAdjustments',
  components: {
    XIcon,
  },
  methods: {
    close() {
      this.$emit('close')
    },
  },
}
</script>

<style lang="stylus" scoped>
.modal__backdrop
  position absolute
  z-index 9999
  top 0
  bottom 0
  left 0
  right 0
  background-color rgba(#fff, .5)
  display flex
  justify-content center
  align-items center

.modal__container
  border-radius $radius
  border 2px solid $border-color
  background-color #fff
  display flex
  flex-direction column
  justify-content space-between
  width 50rem
  padding 20px  15px
  position relative

.modal__title
  font-size 1.8rem
  text-align center
  font-weight 400

.modal__close
  position absolute
  top 1rem
  right @top
  background-color transparent
  cursor pointer
  border-radius 50%
  padding-top .2rem
  hoverTransition()

.content__form
  display grid
  justify-content space-around
  grid-template-rows repeat(2,1fr)
  grid-template-columns repeat(2,25%)
  row-gap 2rem
  column-gap 4rem
  margin 4rem 0

.form__input
  display flex
  flex-direction column
  justify-content center

.input__label
  text-align center
  font-size 1.4rem
  margin-bottom .3rem

.input__field
  width 100%
  border 1px solid $border-color
  border-radius $radius
  padding .3rem .5rem
  font-size 1.3rem
  outline none
  transition all .1s linear
  addShadow()

.content__footer
  justifyCenter()
  justify-content center

.footer__confirm
  background-color transparent
  font-size 1.5rem
  border 2px solid $border-color
  border-radius $radius
  hoverTransition()
  addShadow( )
  padding .5rem .3rem
  width 35%
  cursor pointer

.modal--fade-enter, .modal--fade--leave-active
  opacity 0
  top -10px

.modal--fade-enter-active, .modal--fade-leave-active
  transition all .4s ease

</style>
