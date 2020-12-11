<template>
<div class="modal is-active">
    <div class="modal-background"></div>
    <div class="modal-card">
        <header class="modal-card-head">
            <p class="modal-card-title">Pick a Paint Color</p>
            <button class="delete" aria-label="close" @click="toggle"></button>
        </header>
        <section class="modal-card-body">
            <p class="panel-tabs">
                <a class="is-active">Gloss</a>
                <a>Metallic</a>
                <a>Custom</a>
            </p>
            <div class="columns is-gapless" v-for="i in 3" :key="i">
                <div class="column" v-for="i in 20" :key="i">
                    <button class="button" @click="toggle" ></button>
                </div>
            </div>
        </section>
        <footer class="modal-card-foot">

        </footer>
    </div>
</div>
</template>

<script>
export default {
  name: 'PaintPicker',
  props: {
    list: Array
  },
  methods: {
      toggle() {
          this.$el.classList.toggle('is-active')
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
