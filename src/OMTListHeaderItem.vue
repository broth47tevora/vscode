<template>
  <span v-on="$listeners">
    <span :class="{currentSort:active}">
      <slot></slot>
    </span>
    <OMTIcon v-if="!active" class="sortIcon" icon="sort" size="lg" />
    <OMTIcon v-else-if="sortOrder === 'ascending'" class="sortIcon" icon="sort-up" size="lg" />
    <OMTIcon v-else class="sortIcon" icon="sort-down" size="lg" />
  </span>
</template>

<script>
export default {
  name: "OMTListHeaderItem",
  props: {
    active: Boolean,
    sortOrder: String,
  },
};
</script>

<style lang="stylus" scoped>
.header {
  &-col {
    display: flex;
    justify-content: space-between;
  }
}

.currentSort{
  font-weight: bold;
}
</style>