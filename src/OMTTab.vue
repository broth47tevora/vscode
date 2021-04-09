<template>
  <div
    :class="[{ 'OMTTab': $children.length }, { 'spaced': spaced }]" v-if="isActive"
    v-bind="$attrs"
    v-on="$listeners"
    @click="value => $emit('click', value)"
  >
    <slot></slot>
  </div>
</template>

<script>
  export default {
    name: 'omt-tab',

    props: {
      name: {
        type: String,
        requierd: true,
      },
      selected: {
        type: Boolean,
        default: false,
      },
      spaced: Boolean
    },

    data() {
      return {
        isActive: this.selected,
      }      
    },
  }
</script>

<style lang="stylus" scoped>
  .OMTTab
    height: auto
    padding: 0
    line-height: 2rem
    min-height: 20rem

  .spaced
    padding-top: 1rem
</style>
