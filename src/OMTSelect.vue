<template>
  <div class="OMTSelect">
    <label class="OMTSelect-label" v-if="label">
      {{ label }}
    </label>

    <div class="OMTSelect-wrap">      
      <OMTInput
        :placeholder="placeholder"
        :show-alt-text="!!selectedOptionsSummary"
        :disabled="disabled"
        @click="expandItems(true)"
        @blur="onBlur"
      >
        <template #altText>
          <span
            class="placeholder"
            tabindex="0"
            @click="expandItems(!expanded)"
            @blur="onBlur"
          >
            {{ selectedOptionsSummary }}
          </span>
        </template>

        <template #sufix>
          <div @click="expandItems(!expanded)">
            <OMTIcon icon="chevron-down" size="xs" />
          </div>
        </template>
      </OMTInput>
      <div class="ItemsContainer">
        <div v-show="expanded">
          <input
            type="text"
            class="SearchBox"
            placeholder="Search here to filter the list"
            v-model="search"
            @blur="onBlur"
          >
        </div>
        <ul :class="['ListItems', { 'ListItems--expanded': expanded }]">
          <li
            v-for="item in items"
            tabindex="0"
            :class="['Item', { 'Item--selected': item.selected, 'Item--multi': multiple }]"
            :key="item[trackBy]"
            @click="selectItem(item)"
            @blur="onBlur"
          >
            <template v-if="multiple">
              <OMTIcon v-if="item.selected" class="Item-icon Item-icon--checked" icon="check-circle" />
              <OMTIcon v-else class="Item-icon" icon="check-circle" />

            </template>
            <span >{{ item[displayBy] }}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
import deepClone from 'lodash/cloneDeep';

export default {
  name: 'OMTSelect',
  inheritAttrs: false,

  props: {
    label: {
      type: String,
      default: ''
    },
    options: {
      type: Array,
      default: () => [],
    },
    disabled: Boolean,
    placeholder: String,
    value: [String, Number, Object],
    displayBy: String, // Mandatory when multiple is true
    trackBy: String, // Mandatory when multiple is true
    multiple: Boolean,
    closeOnSelect: Boolean, // Only available when multiple is false.
  },

  data() {
    return {
      expanded: false,
      optionSelected: [],
      search: '',
      listItems: [],
      selectedOptionsSummary: undefined,
    }
  },

  created() {
    this.preSelectOptions(this.options);
  },

  watch: {
    options(options) {
      this.preSelectOptions(options);
    },
  },

  computed: {
    items() {
      if (this.search && this.displayBy) {
        const parsedValue = this.search.toLowerCase();

        return this.listItems.filter(l => l[this.displayBy].toLowerCase().includes(parsedValue));
      }

      return this.listItems;
    },

    selectedValue() {
      if (this.optionSelected && this.displayBy) {
        return this.optionSelected[this.displayBy];
      }

      return this.value;
    },
  },

  methods: {
    onBlur() {
      setTimeout(() => {
        const keepFocus = document.activeElement.closest('.ItemsContainer');

        if (!keepFocus) {
          this.expandItems(false);
        }
      }, 50);
    },

    expandItems(value = true) {
      this.expanded = value;
    },

    preSelectOptions(options) {
      const preSelectedOptions = options.filter(i => i.selected);
      const totalPreSelected = preSelectedOptions.length;

      if (totalPreSelected) {
        if (this.multiple) {
          preSelectedOptions.forEach(item => {
            const isExist = this.optionSelected.findIndex(l => l[this.trackBy] === item[this.trackBy]);
            (isExist === -1) && this.optionSelected.push(item);
          });

          this.selectedOptionsSummary = preSelectedOptions.map(l => l[this.displayBy]).join(', ');
          this.$emit('input', preSelectedOptions);
        }
        else {
          this.optionSelected = preSelectedOptions[0];
          this.selectedOptionsSummary = this.selectedValue;
          this.$emit('input', preSelectedOptions[0]);
        }

      }

      this.listItems = deepClone(options);
    },

    toggleItems() {
      setTimeout(() => {
        this.expanded = false;
      }, 200)
    },

    selectItem(item, isPreselected = false) {
      if (this.multiple) {
        const index = this.optionSelected.findIndex(l => l[this.trackBy] === item[this.trackBy]);

        if (index > -1) {
          this.optionSelected.splice(index, 1);
        } else {
          this.optionSelected.push(item);
        }

        this.$emit('input', this.optionSelected);
      } else {
        this.listItems.forEach(i => i.selected && (i.selected = false))
        this.optionSelected = [];
        this.optionSelected.push(item);
        this.$emit('input', item);
      }

      this.selectedOptionsSummary = this.optionSelected.map(l => l[this.displayBy]).join(', ');
      this.search = this.selectedValue;

      if (isPreselected) {
        return;
      }

      this.$set(item, 'selected', !item.selected);
      this.closeOnSelect && this.toggleItems();
    },
  }
}
</script>
<style lang="stylus" scoped>
  .OMTSelect
    display: flex
    flex-direction: column
    padding: 5px 0

    &-wrap
      display: flex
      flex-direction: column
      width: 100%
      position: relative

      & .OMTInput
        cursor: pointer
        padding: 0

    & p
      color: red

    &-label
      font-weight: 700
      padding: 7px 10px 7px 0

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
    
  .ItemsContainer
    position: absolute
    background-color: white
    top: 2rem
    left: 0
    right: 0
    z-index: 999
    height: auto
    overflow-y: auto
    max-height: 300px

    & .ListItems
      list-style: none
      border: none
      border-radius: 3px
      border-top: none
      border-top-left-radius: 0;
      border-top-right-radius: 0;
      box-shadow: 0 2px 2px rgba(175, 175, 175, 0.16)
      height: 0
      overflow: hidden
      display: none

      &--expanded
        height: inherit
        overflow: auto
        display: block
        border: 1px solid #cacaca

    & .Item
      line-height 18px
      padding: 8px      
        
      &:focus
        outline-color: #d0d0d0

      &--selected
      &:hover
        cursor: pointer
        font-weight: 700
        background-color: #e4e4e4
        border-bottom: 1px solid white

      &-icon  
        color: #e4e4e4

        &--checked
          color: #45bc98

      &--multi
        display: flex
        align-items: center
        justify-content: flex-start

        & span
          margin-left: 8px

  .SearchBox
    border: 1px solid #cacaca
    border-bottom-right-radius: 0;
    border-bottom-left-radius: 0;
    border-bottom: none;
    padding: 5px;
    font-family: 'Lato', Arial, Helvetica, sans-serif
    width: 100%

    &:focus
      outline: none
      border: 1px solid #cacaca
      border-bottom: none

@media screen and (min-width: 1023px)
  .OMTSelect
    align-items: center
    flex-direction: row

    &-label
      flex-basis: 250px
</style>