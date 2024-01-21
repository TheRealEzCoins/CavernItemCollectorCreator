<script>
import { ref } from 'vue';
import Items from './assets/items.json';


export default {
  data() {
    return {
      Items,
      selected_option: null,
      name: "",
      lore: "",
    };
  },
  computed: {
    itemIDs() {
      return this.Items.map(item => item.name).sort();
    },
  },
  props: {},
  methods: {
    formatItemName(name) {
      return name.split('_')
        .map(word => word.charAt(0).toUpperCase() + word.slice(1))
        .join(' ');
    },
    getIcon(name){
      console.log(name)
      return `./src/assets/items/${name}.png`; //you may need change this according to your image path in the project
    },
  },
};
</script>

<template>
  <div>
    <div style="display: flex; align-items: center;">
      <img v-if="selected_option" :src="getIcon(selected_option)" alt="?" style="height:30px; margin-right: 10px;">
      <select v-model="selected_option">
        <option v-for="name in itemIDs" :key="name" :value="name">
          {{ formatItemName(name) }}
        </option>
      </select>
    </div>
    <div style="align-items: center;">
      <input v-model="name" placeholder="Name">
    </div>
    <div>
      <textarea v-model="lore" placeholder="Lore"></textarea>
    </div>
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
