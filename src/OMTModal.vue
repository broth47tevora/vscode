<template>
  <div :class="$style.modal" v-if="show">
    <div :class="$style.container">
      <h3 :class="$style.caption" v-if="caption">
        {{ caption }}
      </h3>

      <div :class="$style.body">
        <slot></slot>
      </div>

      <div :class="$style.actions" v-if="$slots.actions">
        <slot name="actions"></slot>
      </div>      
    </div>
  </div>
</template>

<script>
export default {
  name: 'OMTModal',

  props: {
    caption: String,
    show: Boolean,
  },
}
</script>

<style lang="stylus" module>
  .modal
    position: absolute
    left: 0
    right: 0
    top: 0
    bottom: 0
    background-color: rgba(255,255,255, 0.5)
    z-index: 1000

  .container
    margin: 0 auto
    top: 10%
    left: 50%
    transform: translateX(-50%)
    position: fixed
    width: 350px
    box-shadow: 0 0 40px 1px rgba(0, 0, 0, 0.3)
    background: #fff
    display: flex
    flex-direction: column
  
  .body
    max-height: 400px
    overflow: auto


  .caption
    padding: 1rem
    font-size: 1.2rem
    background: #007dc1
    color: whitesmoke
    font-weight: 400

  .actions
    display: flex;
    justify-content: flex-end
    background-color: #f2f2f2
    border-width: 1px 0
    border-style: solid
    border-color: #dadada #dddddd
    padding: 1rem 0
    margin-top: auto

  @media screen and (min-width: 760px)
    .container
      width: 720px

    .body
      max-height: 500px
</style>