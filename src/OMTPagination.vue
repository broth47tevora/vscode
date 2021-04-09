<template>
  <div class="OMTPagination">
    <ul class="OMTPagination-list">
      <li>
        <select v-model="itemsPerPage" @change="$emit('change', {page: selectedPage, items: itemsPerPage})">
          <option v-for="item in ITEMS_PER_PAGE_OPTIONS" :key="item" :value="item">{{ item }}</option>
        </select>
      </li>
      <li v-for="page in pages" :key="page">
        <button :class="[ { 'active': page === selectedPage } ]" @click="$emit('change', {page, items: itemsPerPage})">{{ page }}</button>
      </li>
    </ul>
  </div>
</template>

<script>

const ITEMS_PER_PAGE_OPTIONS = [10, 25, 50, 100];

export default {
  name: "OMTPagination",

  props: {
    pages: {
      required: false,
      default: 1,
    },
    selectedPage: {
      required: false,
      default: 1
    }
  },

  data() {
    return {
      itemsPerPage: 10,
      ITEMS_PER_PAGE_OPTIONS
    }
  }
}
</script>

<style lang="stylus">
.OMTPagination
  height: 5rem

  & select
    border-radius: 2px
    padding: 1px 4px;
    border: 1px solid #AAAAAA
    background-color: #FFFFFF

  &-list
    margin: 0
    padding: 0.3rem 0

    & li 
      list-style-type: none
      float: left
      padding: 0 0.3rem
    
      & button
        padding: 1px 4px;
        border-radius: 2px
        border: 1px solid #AAAAAA
        background-color: #FFFFFF
        color: #5e5e5e
        cursor: pointer

        &.active
          color: #FFFFFF
          background-color: #007dc1
</style>