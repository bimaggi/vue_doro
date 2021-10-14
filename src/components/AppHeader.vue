<template functional>
  <header
    class="header"
    key="header">
    <div class="header__logo">
      <h1 class="logo__title">VueDoro</h1>
      <img
      class="logo__image"
      src="@/assets/logo.png"
      alt="logo">
  </div>
  </header>
</template>

<script>
export default {
  name: 'AppHeader',
}
</script>

<style lang="stylus">
.header
  alignCenter()
.header__logo
  alignCenter()
  gap 1rem
.logo__image
  width 3.8rem
.logo__title
  font-size 2rem
</style>
