<template>
  <div class="OMTMatchByField">
    <h3 class="OMTMatchByField-title">Perform your custom match</h3>
    <div class="OMTMatchByField-fields">
      <OMTSelect
        closeOnSelect
        displayBy="label"
        placeholder="Field to Match"
        trackBy="id"
        :options="fields"
        :key="filters.length"
        @input="event => match.field = event.value"
      />

      <OMTSelect
        closeOnSelect
        displayBy="label"
        placeholder="Match action"
        trackBy="id"
        :options="MATCH_ACTIONS"
        :key="randomId"
        @input="event => match.type = event.value"
      />

      <OMTInput
        placeholder="Value"
        v-model="match.value"
      />

      <OMTButton
        inline
        class="OMTMatchByField-action"
        @click="addNewFilterRow"
      >
        <OMTIcon icon="plus" />
      </OMTButton>
    </div>

    <OMTMatchByFieldRow
      v-for="(filter, index) in filters"
      :key="index"
      :field="filter.field"
      :action="filter.type"
      :value="filter.value"
      @remove="filters.splice(index, 1)"
    />
  </div>
</template>

<script>

const MATCH_ACTIONS = [
  { id: 1, label: 'Equals', value: 'eq'},
  { id: 2, label: 'Starts With', value: 'sw'},
  { id: 3, label: 'Regex', value: 'regex'},
];

export default {
  name: "OMTMatchByField",

  props: {
    fields: {
      type: Array,
      required: true,
    },

    items: Array
  },

  data() {
    return {
      MATCH_ACTIONS,
      match: {},
      filters: [],
      randomId: this.randomNumber(),
    };
  },

  mounted() {
    if (this.items) {
      this.items.forEach((item) => this.filters.push(item));
    }
  },

  methods: {
    addNewFilterRow() {
      const { field, type, value} = this.match;

      this.filters.push({ type, field, value });
      this.match = {};
      this.randomId= this.randomNumber();
      this.$emit('change', this.filters);
    },

    randomNumber() {
      return Math.round(Math.random() * 100);
    },
  }
}
</script>

<style lang="stylus" scoped>
.OMTMatchByField
  &-title
    font-weight: 700
    font-size: 13px
    padding: 7px 10px 7px 0

  &-action
    margin-bottom: 0.4em
    padding: 0 0.4em
    cursor: pointer
    align-self: flex-end
  
  &-fields
    display: flex
    align-items: center

    & > div
      flex: 1      
      margin-right: 10px
      padding: 0 !important
</style>