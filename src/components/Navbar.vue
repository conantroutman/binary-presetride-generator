

<template>
<nav class="navbar is-primary" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="https://bulma.io">
      <h1 class="is-capitalized has-text-weight-bold">PresetRide Generator</h1>
    </a>

    <a role="button" class="navbar-burger burger" aria-label="menu" aria-expanded="false" data-target="navbarBasicExample">
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
    </a>
  </div>

  <div id="navbarBasicExample" class="navbar-menu">
    <div class="navbar-start">
      <a class="navbar-item" href="https://nfs.fandom.com/wiki/Category:Visual_Customisation_(Need_for_Speed:_Most_Wanted)" target="_blank">
        Visual Reference
      </a>

      <a class="navbar-item" href="https://github.com/SpeedReflect/Binary/releases" target="_blank">
        Binary
      </a>
    </div>
  </div>
</nav>
</template>

<script>
export default {
  name: 'Navbar',
  props: {

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
