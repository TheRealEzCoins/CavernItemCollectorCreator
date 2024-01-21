<template>
  <div id="app">
    <div class="item-creation-pages">
      <ItemCreationPage v-for="n in 9" :key="n" :collapsed="true" :selected_small_number="n" @click="toggleCollapse(n)" />
    </div>
    <button @click="generateItems">Generate</button>
  </div>
</template>
<script>
import ItemCreationPage from './components/pages/ItemCreationPage.vue';

export default {
  components: {
    ItemCreationPage
  },
  data() {
    return {
      items: Array.from({length: 9}, () => true),
    }
  },
  methods: {
    toggleCollapse(index) {
      this.items[index - 1] = !this.items[index - 1];
    },
    generateItems() {
      // Add generation logic here
    }
  }
};
</script>

<style scoped>
#app {
  height: 100vh;
  display: flex;
  flex-direction: column;
}
.item-creation-pages {
  display: flex;
  flex-wrap: wrap;
}
</style>
