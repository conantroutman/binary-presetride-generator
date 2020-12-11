<template>
<div class="field is-horizontal">
    <div class="field-label is-normal">
        <label class="label" for="tint_select">{{ label }}</label>
    </div>
    <div class="field-body">
        <div class="field">
        <div class="control">
            <div class="select">
            <select name="tint" id="tint_select">
                <option value="" v-if="stock" selected>{{ stock }}</option>
                <option v-for="item in list" v-bind:key="item" :value="item.id">{{ item.id }}</option>
            </select>
            </div>
        </div>
        </div>
    </div>
    </div>
</template>

<script>
export default {
  name: 'bulma-select',
  props: {
    label: String,
    list: Array,
    stock: String,
    value: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
