<template>
  <div class="OMTMatchByFieldRow">
    <div>
      <p class="OMTMatchByFieldRow-line">{{field}}</p>
    </div>
    <div>
      <p class="OMTMatchByFieldRow-line">{{action}}</p>
    </div>
    <div>
      <p class="OMTMatchByFieldRow-line">{{value}}</p>
    </div>

    <span class="OMTMatchByFieldRow-actions">
      <OMTButton
        inline
        class="OMTMatchByField-action"
        @click="$emit('remove')"
      >
        <OMTIcon icon="minus" />
      </OMTButton>
    </span>
  </div>
</template>

<script>
export default {
  name: 'OMTMatchByFieldRow',

  props: {
    field: {
      type: String,
      required: true,
    },

    action: {
      type: String,
      required: true,
    },

    value: {
      type: String,
      required: true,
    },

  }
}
</script>

<style lang="stylus">
  .OMTMatchByFieldRow
    display: flex
    align-items: center
    margin-top: 1rem

    & > div
      flex: 1      
      margin-right: 10px
      padding: 0 !important

    &-line
      background-color: #dadada
      width: 100%
      padding: 0.4rem
      border-radius: 3px

</style>