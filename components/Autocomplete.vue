<template>
  <div @focusin="handleFocusIn" @focusout="handleFocusOut">
    <input
      id="txtSearch"
      class="text-center form-control"
      v-model="searchQuery"
      @input="handleInputChange"
      autocomplete="off"
    />
    <ul v-if="isFocused && filteredHistory.length && showSuggestWhenNotEqual()" class="border border-gray-300 rounded-md bg-white px-0">
      <li v-for="item in filteredHistory" :key="item" @click="selectItem(item)" class="px-4 py-2 border-b cursor-pointer">
        {{ item }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: this.inputSearch,
      isFocused: false,
    };
  },
  computed: {
    filteredHistory() {
      return this.historySearchList.filter(item => item.toLowerCase().includes(this.searchQuery.toLowerCase()));
    },
  },
  methods: {
    handleInputChange() {
      this.$emit('input-change', this.searchQuery);
    },
    selectItem(item) {
      this.$emit('item-selected', item);
      this.searchQuery = item;
    },
    showSuggestWhenNotEqual() {
      return !(this.filteredHistory.length === 1 && this.searchQuery.toLowerCase() === this.filteredHistory[0].toLowerCase());
    },
    handleFocusIn() {
      this.isFocused = true;
    },
    handleFocusOut() {
      // Use setTimeout to delay hiding the suggestion list
      setTimeout(() => {
        this.isFocused = false;
      }, 200);
    },
  },
  props: {
    historySearchList: {
      type: Array,
      default: () => [],
    },
    inputSearch: {
      type: String,
      required: true,
    },
  },
};
</script>

<style>
  li {
    list-style-type: none;
  }

  li:hover {
    background-color: #d1e2ff;
  }
</style>
  