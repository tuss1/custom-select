<template>
  <div id="app">
    <p>Реализовать кастомный select со следующими возможностями:
      <ul>
        <li>
          - поддержка v-model
        </li>
        <li>
            - управление выбором элемента с помощью клавиатуры
        </li>
        <li>
            - вывод полупрозрачного плейсхолдера, если значение не выбрано
        </li>
        <li>
            - возможность кастомизировать вывод значения и элементов списка извне
        </li>
      </ul>
    </p>
    <select
      v-model="chosenFramework"
      @input="$emit('input', $event.target.value)"
    >
      <option
        v-for="option in frameworks"
        :key="option.key"
        :value="option.value"
        >{{ option.text }}</option
      >
    </select>

    <VSelect
      v-model="chosenFramework"
      :options="frameworks"
    ></VSelect>

    <VSelect
      v-model="chosenFramework"
      placeholder="полупрозрачного плейсхолдера"
      :options="frameworks"
    >
      <template
        v-slot:select-input="slotProps"
        v-bind:chosen-framework="chosenFramework"
      >
        <div style="height:2em;margin-right:20px">
          <span v-if="chosenFramework" v-html="chosenFramework"></span>
          <span style="color: rgba(50,50,50,.3)" v-else v-html="slotProps.placeholder"></span>
        </div>
        <div style="position:absolute;right:0;top:0">X</div>
      </template>

      <template v-slot:select-list v-bind:frameworks="frameworks">
      </template>
    </VSelect>
  </div>
</template>

<script>
import VSelect from "./components/VSelect.vue";

export default {
  name: "App",
  data() {
    return {
      chosenFramework: undefined,
      frameworks: [
        {
          value: "vanilla",
          text: "Vanilla JavaScript",
          icon: "devicon-javascript-plain",
        },
        {
          value: "vue",
          text: "Vue",
          icon: "devicon-vuejs-plain",
        },
        {
          value: "angular",
          text: "Angular",
          icon: "devicon-angularjs-plain",
        },
        {
          value: "react",
          text: "React",
          icon: "devicon-react-original",
        },
        {
          value: "backbone",
          text: "Backbone",
          icon: "devicon-backbonejs-plain",
        },
      ],
    };
  },
  components: {
    VSelect,
  },
};
</script>
