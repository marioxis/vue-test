<template>
  <div>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/micasino.png" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <FormComponent :fieldDefinitions=fieldDefinitions />
</template>
<script setup lang="js">
import FormComponent from './components/FormComponent.vue';

let fieldDefinitions = [
  {
    id: 'name',
    label: 'Nombre',
    type: 'Text',
    required: true,
  },
  {
    id: 'options',
    label: 'Options',
      type: 'SelectUnselect',
      options: [
        {
          id: '1',
          label: 'Option 1',
        },
        {
          id: '2',
          label: 'Option 2',
        },
        {
          id: '3',
          label: 'Option 3',
        },
      ],
    required: true,
  }
];
</script>
<style>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #e3bd00aa);
}

body {
  background-color: #222;
  color: white;
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

button {
  background-color: #444;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #555;
}

</style>