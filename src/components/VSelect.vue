<template>
  <div :class="{ select: true, 'select-focused': focused }">
    <div 
      class="select-splash" 
      v-if="listOpened" 
      @click.stop="listOpened = false"
    ></div>

    <div 
      class="select-input" 
      @click.prevent="listOpened = !listOpened"
    >
      <slot 
        name="select-input" 
        v-bind:placeholder="placeholder"
      >{{value ? value : placeholder}}</slot>
    </div>

    <div class="select-list" v-if="listOpened">
      <VSelectList 
        v-model="value_" 
        :options="options" 
        @input="handleSelectItem"
      ></VSelectList>
    </div>

    <select
      v-model="value_"
      ref="native-select"
      @focus="focused = true"
      @blur="focused = false"
      @input="handleSelectItem($event.target.value)"
      @keydown.enter="handleEnterPressed"
    >
      <option 
        v-for="option in options" 
        :key="option.key" 
        :value="option.value"
      >{{ option.text }}</option>
    </select>
  </div>
</template>

<script>
import VSelectList from "./VSelectList.vue";

export default {
  props: {
    "options": {
      default: () => [],
    },
    "value": {
      default: '',
    },
    "placeholder": {
      default: 'Choose an item',
    },
  },
  data() {
    return {
      focused: false,
      listOpened: false,
      value_: this.value,
    };
  },
  components: {
    VSelectList,
  },
  watch: {
    value: function (newVal) {
      this.value_ = newVal;
    },
    listOpened: function (newVal) {
      this.$nextTick(() => {
        if (!newVal) this.$refs["native-select"].focus();
      });
    },
  },
  methods: {
    handleEnterPressed() {
      this.listOpened = !this.listOpened;
    },
    handleSelectItem(item) {
      this.$emit("input", item);
      this.listOpened = false;
    },
  },
};
</script>

<style scoped>
select {
  position: absolute;
  left: -20000px;
  top: 0;
}

.select {
  min-height: 20px;
  min-width: 150px;
  position: relative;
  display: inline-block;
  box-sizing: border-box;
  cursor: default;
  margin: 0em;
  font: 400 13.3333px Arial;
  border-radius: 0px;
  border-width: 1px;
  border-style: solid;
}

.select-focused {
  outline: -webkit-focus-ring-color auto 1px;
}

.select-splash {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background: transparent;
}
</style>