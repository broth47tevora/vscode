<template>
  <form class="OMTForm" @submit.prevent="send">
    <h3 class="OMTForm-header" v-if="caption">
      <span>{{ caption }}</span>
      <span class="OMTForm-edit">
        <slot name="edit" class=""></slot>
      </span>
    </h3>
    <div :class="['OMTForm-body', { 'OMTForm-body--borderless': embebed }]">
      <slot></slot>
    </div>

    <div class="OMTForm-actions" v-if="$slots.actions">
      <slot name="actions"></slot>
    </div>
  </form>
</template>

<script>
export default {
  name: 'OMTForm',

  props: {
    caption: String,
    embebed: Boolean
  },

  methods: {
    send() {
      this.$emit('submit');
    }
  }
}
</script>

<style lang="stylus" scoped>
  .OMTForm
    &-header
      margin: 0
      padding: 0 20px
      border: 1px solid #dddddd
      border-bottom-width: 0
      border-top-left-radius: 4px
      border-top-right-radius: 4px
      background: #f2f2f2
      color: #5e5e5e
      line-height: 3rem
      display: flex
      line-height: 3rem
      font-weight: 400
      font-size: 17px

    &-edit
      margin-left: auto

      & button
        line-height: 0

    &-body
      padding: 30px
      border: 1px solid #dddddd
      border-radius: 4px
      border-top-left-radius: 0
      border-top-right-radius: 0
      border-bottom-left-radius: 0
      border-bottom-right-radius: 0
      border-bottom-color: #d2d2d2
      background: #f9f9f9
      box-shadow: 0 1px 1px rgba(175, 175, 175, 0.16)

      &--borderless
        border: none
        box-shadow: none
        background: none

    &-actions
      display: flex
      justify-content: flex-end
      background-color: #f2f2f2
      border-width: 1px
      border-style: solid
      border-color: #dadada #dddddd
      padding: 1rem 0
      border-radius: 0 0 3px 3px
      box-shadow: 0 1px 1px rgba(175, 175, 175, 0.16)
      border-top: none
</style>