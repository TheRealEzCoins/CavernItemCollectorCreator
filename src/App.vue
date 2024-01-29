<template>
  <div id="app">
    <div style="position: sticky; top: 0;">
      <details>
        <summary>Tag Creation</summary>
        <div class="item-creation-pages">
          <div style="display:flex; flex-direction:column;">
            <input type="text" placeholder="Tag name" v-model="tagName">

            <div>
              <input type="radio" id="radio1" v-model="selectedRadio" value="3DItem">
              <label for="radio1">3D Item</label>
              <input type="radio" id="radio2" v-model="selectedRadio" value="Tag">
              <label for="radio2">Tag</label>
              <input type="text" placeholder="Inventory Slot" v-model="inventorySlot">
              <input type="text" v-if="selectedRadio === '3DItem'" placeholder="Custom Model Data" id="custom-model-data">
            </div>
            <input type="text" placeholder="Emoji" maxLength="2" id="tag-emoji">
          </div>
        </div>
      </details>
      <details>
        <summary>Item Creation</summary>
        <div class="item-creation-pages">
          <ItemCreationPage v-for="n in 9" :key="n" :collapsed="true"
                            :selected_small_number="n"
                            @click="toggleCollapse(n)"
                            ref="itemRefs" /> <!-- Add ref here -->
        </div>
      </details>
      <button @click="generateItems">Generate</button>

      <div v-if="showPopup" class="popup">
        <h3>Preview</h3>
        <pre style="overflow-y:auto; height:100px;">{{ previewData }}</pre>
        <a :href="downloadUrl" download="data.txt">Download</a>
        <button @click="closePopup">Close</button>
      </div>
    </div>
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
      selectedRadio : null,
      tagName: '',
      inventorySlot: '',
      customModelData: '',
      emoji: '',
      showPopup: false,
      downloadUrl: null,
      allItems: [],
    }
  },
  computed: {
    previewData() {
      return`${this.tagName}:
  name: '${this.tagName}'
  material: ${this.getMaterial(this.selectedRadio)}
  slot: ${this.inventorySlot}
  ${this.getType(this.selectedRadio)}
  ${this.allItems.map(item => this.returnAsText(item))}
          `; // Modifed here
    }
  },
  methods: {
    getMaterial(selectedRadio) {
      if(selectedRadio === '3D radio') {
        return 'FEATHER'
      } else {
        return 'NAME_TAG'
      }
    },
    getType(selectedRadio) {
      if(selectedRadio === '3D radio') {
        return `custom_model_data: ${this.customModelData}\n\
        command: o give {PLAYER} ${this.tagName} 1`
      } else {
        return `command: lp user {PLAYER} permission set deluxetags.tag.${this.tagName} server=towny`
      }
    },

    toggleCollapse(index) {
      this.items[index - 1] = !this.items[index - 1];
    },
    returnAsText(item) {
      const lore = item.lore.join('. ');
      return `
  '${item.item_number}'\n\
    material: ${item.material}\n\
    name: '${item.name}'\n\
    lore:\n\
    - '&8${lore.split('. ').join("\n    - '&8")}'\n\
    - '&8      '\n\
    - '&e${item.selected_month} ${new Date().getFullYear()} Collectors Item'`
    },

    generateItems() {
      this.allItems = this.$refs.itemRefs.map((ref) => ref.item); // Modify this to get Item from each ItemCreationPage
      const blob = new Blob([this.previewData], { type: 'text/yml' });
      this.downloadUrl = URL.createObjectURL(blob);
      this.showPopup = true;
    },
    closePopup() {
      this.showPopup = false;
    },
  }
};
</script>

<style scoped>
#app {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.popup {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background: dimgray;
  padding: 20px;
  border: 1px solid black;
  border-radius: 10px; /* Added this for a more professional look */
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.15); /* Added this for a more professional look */
}

.item-creation-pages {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

</style>
