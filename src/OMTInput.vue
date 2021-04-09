<template>
  <div class="OMTInput">
    <label class="OMTInput-label" v-if="label">
      {{ label }}
      <slot name="tooltip">
      </slot>
    </label>

    <template v-if="textarea">
      <span v-if="disabled" class="OMTInput-element OMTInput-element--read">{{ value }}</span>
      <div v-else class="OMTInput-wrap">
        <textarea
          class="OMTInput-element"
          :value="value"
          @input="updateValue"
          v-bind="$attrs"
          v-on="listeners"
        />
      </div>
    </template>

    <template v-else>
      <span v-if="disabled" class="OMTInput-element OMTInput-element--read">{{ value }}</span>
      <div v-else class="OMTInput-wrap">
        <OMTIcon v-if="icon" :icon="icon" class="OMTInput-icon" size="sm" />
        <input
          :class="['OMTInput-element',  { 'OMTInput-element--rounded': rounded, 'OMTInput-element--icon': icon }]"
          :value="value"
          @blur="blur"
          @input="updateValue"
          v-bind="$attrs"
          v-on="listeners"
        >
        <slot
          name="altText"
          class="placeholder"
          v-if="showAltText"
        >
        </slot>
        <span class="sufix">
          <slot name="sufix"></slot>
        </span>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  inheritAttrs: false,

  props: {
    label: {
      type: String,
      default: ''
    },
    textarea: Boolean,  
    value: [String, Number],
    disabled: Boolean,
    rounded: Boolean,
    showAltText: Boolean,
    icon: String,
  },

  computed: {
    listeners() {
      return {
        ...this.$listeners,
        input: this.updateValue
      }
    }
  },

  methods: {
    updateValue(event) {
      this.$emit('input', event.target.value);
    },
    
    blur() {
      this.$emit('blur', event.target.value)
    }
  },
}
</script>

<style lang="stylus" scoped>
  .OMTInput
    display: flex
    flex-direction: column
    padding: 5px 0

    &:first-child
      padding-top: 0

    &-icon
      position: absolute
      top: 9px
      left: 9px
      color: #CCCCCC
    &-wrap
      display: flex
      flex-direction: column
      width: 100%
      position: relative
      justify-content: center

      & .sufix
        position: absolute
        right: 0
        cursor: pointer
        width: 40px
        text-align: center
        display: block

      & .placeholder
        align-items: center;
        display: flex;
        position: absolute
        left: 2px
        right: 2px
        top: 2px
        bottom: 2px
        padding: 0 5px
        background: white
        user-select: none
        text-overflow: ellipsis
        width: 95%
        white-space: nowrap
        overflow-x: hidden
        overflow-y: hidden

        &:focus
          outline: none

    & p
      color: red

    &-label
      font-weight: 700
      padding: 7px 10px 7px 0
      position: relative

    &-tooltip
      position: absolute
      top: 4px
      left: -12px
      color: #AAAAAA

      &:hover
        color: #777777

    &-element
      font-family: 'Lato', Arial, Helvetica, sans-serif
      flex: 1
      padding: 0.4rem
      line-height: 1rem
      border: 1px solid #cacaca
      background-color: #fff
      box-shadow: inset 0 1px 1px hsla(0,0%,62.7%,.2)
      border-radius: 4px
      width: inherit

      &--read
        border: none
        box-shadow: none
        border-radius: none
        flex: 1
        background-color: transparent

      &--rounded
        border-radius: 17px

      &--icon
        padding-left: 25px

      &:focus
        outline: none
        border-color: #828282

      &::placeholder
        color: #c1c1c1

  @media screen and (min-width: 1023px)
    .OMTInput
      align-items: center
      flex-direction: row

      &-label
        flex-basis: 250px

.border-red
  border-color: red

</style>