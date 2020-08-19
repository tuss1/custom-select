<template>
  <div
    tabindex="0"
    ref="select-list"
    v-focus
    @keydown.down.prevent="handleSelectNextItem"
    @keydown.up.prevent="handleSelectPrevItem"
    @keydown.enter="$emit('input', value_)"
  >
    <slot>
      <ul>
        <li
          :class="{
            'select-list-item': true,
            'select-list-item__active': option.value === value_
          }"
          v-for="option in options"
          :key="option.value"
          @click="handleSelectItem(option.value)"
        >{{option.text}}</li>
      </ul>
    </slot>
  </div>
</template>

<script>
export default {
  props: {
    "value": {
      default: '',
    },
    "options": {
      default: function() {return [{value: 'test'}]}
    }
  },
  data() {
    return {
      value_: this.value || this.options[0].value,
    };
  },
  methods: {
    handleSelectItem(item) {
      this.$emit("input", item);
      this.$destroy();
    },
    handleSelectNextItem() {
      let index = this.options.findIndex(
        (option) => option.value === this.value_
      );

      if (!~index) index = 0;

      const nextIndex = (index + 1) % this.options.length;
      this.value_ = this.options[nextIndex].value;
    },
    handleSelectPrevItem() {
      let index = this.options.findIndex(
        (option) => option.value === this.value_
      );

      if (!index) {
        index = 5;
      }

      const nextIndex = (index - 1) % this.options.length;
      this.value_ = this.options[nextIndex].value;
    },
  },
  watch: {
    value: function (newVal) {
      this.value_ = newVal;
    },
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus();
      },
    },
  },
};
</script>

<style>
.select-list {
  position: absolute;
  width: 100%;
  background: white;
  border: solid 1px black;
}
.select-list ul {
  display: block;
  /* width: 100%; */
  list-style-type: disc;
  margin-block-start: 0;
  margin-block-end: 0;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
  padding-inline-start: 0px;
}
.select-list-item {
  list-style: none;
  padding-inline-start: 2px;
}
.select-list-item:hover,
.select-list-item__active {
  background: green;
}
</style>