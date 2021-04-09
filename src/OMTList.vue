<template>
  <div class="List">
    <div class="List-navigation">
      <OMTInput
        rounded
        placeholder="Search..."
        icon="search"
        v-model="search"
        @input="onSearch"
      >
        <template #sufix>
          <OMTIcon v-if="search" icon="times-circle" class="search-clear" size="xs" @click="clearSearch" />
        </template>
      </OMTInput>
      <div class="List-pagination">
        <template v-if="showPagination">
          <slot name="pagination"></slot>
        </template>
      </div>  
    </div>
    <div class="container">
      <div class="Filters">
        <ul class="Filters-list">
          <li class="Filters-listItem">
            <div>
              <button :class="['Filters-link', { 'Filters-link--active': 'Total' === activeFilter }]" @click.prevent="filterSelected('Total')" >
                Total 
                <span class="Filters-count">{{ totalItems || 0 }}</span>
              </button>
            </div>
          </li>
          <li class="Filters-listItem" v-if="search">
            <div>
              <button :class="['Filters-link', { 'Filters-link--active': 'Search' === activeFilter }]" @click.prevent="filterSelected('Search')">
                {{ search }} 
                <span class="Filters-count">{{ totalSearch || 0 }}</span>
              </button>
            </div>
          </li>
        </ul>
      </div>
      <div class="LineItem">
        <slot name="list-header">
        </slot>
        <div>
          <ul class="listItems">
            <template v-show="listItems.length">
              <slot name="listItems"></slot>
            </template>
          </ul>
        </div>

        <div v-show="!listItems.length" class="emptyList">
          <img :src="empty" alt="empty">
        </div>
      </div>
    </div>      
  </div>
</template>

<script>
import empty from '@/assets/empty-list.png';

export default {
  name: 'OMTList',

  props: {
    listItems: {
      type: Array,
      required: true,
    },
    totalItems: {
      type: Number,
      required: false,
    },
    totalSearch: {
      type: Number,
      required: false,
    },
    showPagination: {
      type: Boolean,
      required: false,
    }
  },

  data() {
    return {
      activeFilter: 'Total',
      search: '',
      empty,
    }
  },

  methods: {
    filterSelected(filter) {
      this.activeFilter = filter;
      this.$emit('change', { activeFilter: filter, searchTerm: this.search });
    },

    onSearch() {
      this.$emit('input', this.search);
      if (this.search) {
        this.filterSelected('Search');
      } else {
        this.filterSelected('Total');
      }
    },
    clearSearch() {
      this.search = '';
      this.filterSelected('Total');
      this.$emit('input', '');
    }
  }
}
</script>

<style lang="stylus" scoped>
  .List
    height: 100%
    min-height: 400px
    background-color: #fff
    margin: 0 auto
    border-width: 1px
    border-style: solid
    border-radius: 3px
    border-color: #dddddd #dddddd #dadada
    box-shadow: 0 2px 0 rgba(175, 175, 175, 0.12)

    .container
      display: flex
      min-height: 349px

    .listItems
      display: flex
      flex-direction: column
    
    &-pagination
      padding-right: 1.5rem

    &-navigation
      border-bottom: 1px solid #dadada
      padding: 10px
      background-color: #f2f2f2
      height: 51px
      display: flex
      flex-direction: row
      justify-content: space-between

      & .OMTInput
        max-width: 187px

      .OMTInput .OMTInput-element
        background: white
        border-radius: 17px
        padding: 0
        padding-left: 10px
        border: 1px solid #cacaca
        border: none
        box-shadow: none
        border: 1px solid red

    .header
      background-color: #f2f2f2

      &-col
      &-actions
        padding: 8px 10px
        border-right: 1px solid #dddddd
        border-bottom: 1px solid #dddddd

      &-col
        flex: 1

  .LineItem
    flex: 1

    & .emptyList
      height: 90%
      display: flex
      justify-content: center
      align-items: center

  .Filters
    width: 20%
    padding-bottom: 30px
    border-top-left-radius: 3px
    border-bottom-left-radius: 3px
    background-color: #f2f2f2
    border-right: 1px solid #dddddd

    &-list
      padding-top: 10px
      padding-bottom: 10px

    &-listItem
      border-top: 0
      list-style: none

      &--grouped
        border-top: 1px solid #ddd

    &-count
      display: block
      position: absolute
      top: 7px
      right: 10px
      color: #999999

    &-heading
      margin: 0
      padding: 15px 10px
      border-bottom: 1px solid #f2f2f2
      color: #5e5e5e
      font-size: 13px
      font-weight: 200
      line-height: 15px
      text-transform: uppercase

    &-link
      width: 100%
      text-align: left
      border: none
      position: relative
      padding: 7px 10px
      overflow: hidden
      font-size: 13px
      line-height: 16px
      text-overflow: ellipsis
      white-space: nowrap
      color: #0074b3
      cursor: pointer

      &:focus
        outline: none

      &:hover
        border-color: #e2e2e2
        background: #e2e2e2
        text-decoration: none

      &--active
        background: #007dc1
        color: #ffffff
        &:hover
          border-color: none
          background: #007dc1

      &--active .Filters-count
        color: #ffffff
  
  .search-clear
    color: #AAAAAA
    
    &:hover
      color: #555555

  @media screen and (min-width: 1023px)
    .List
      max-width: 1024px

</style>
<style lang="stylus">
.List
  .search

    .OMTInput .OMTInput-element
      border: none
      box-shadow: none
</style>