<script>
import { ref } from 'vue';
import Items from '../../assets/items.json';

export default {
  props: {
    selected_small_number: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      Items,
      selected_month: null,
      selected_option: null,
      selected_large_number: null,
      name: "",
      lores: [""],
      textBox1: "",
      textBox2: "",
      current_year: new Date().getFullYear(),
      monthOptions: ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"],
      open: false
    };
  },
  computed: {
    itemIDs() {
      return this.Items.map(item => item.name).sort();
    },
  },
  methods: {
    formatItemName(name) {
      return name.split('_')
          .map(word => word.charAt(0).toUpperCase() + word.slice(1))
          .join(' ');
    },
    getIcon(name) {
      return `./src/assets/items/${name}.png`;
    },
    addLoreField() {
      this.lores.push("");
    },
    fillTextBoxes(){
      const lore = this.lores.join('. ');

      this.textBox1 = `
  '${this.selected_small_number}'\n\
    material: ${this.selected_option.toUpperCase()}\n\
    name: '${this.name}'\n\
    lore:\n\
    - '&8${lore.split('. ').join("\n    - '&8")}'\n\
    - '&8      '\n\
    - '&e${this.selected_month} ${this.current_year} Collectors Item'`;

      this.textBox2 = JSON.stringify({
        name: this.name,
        lore: lore
      }, null, 2);
    },
    toggleOpen() {
      this.open = !this.open;
    }
  },
};
</script>
<template>
  <div class="item-creator">
    <div class="top-bar" @click="toggleOpen">
      <p class="centered-text">
        <span class="bordered-text">
          {{open ? `Collapse ${selected_small_number}` : `Expand ${selected_small_number}`}}
        </span>
      </p>    </div>
    <div class="flex-container">
      <div class="content" v-show="open">
        <div class="align-center">
          <div class="item-detail">
            <img v-if="selected_option" :src="getIcon(selected_option)" alt="?" class="item-icon">
            <select v-model="selected_option" class="item-select">
              <option v-for="name in itemIDs" :key="name" :value="name">
                {{ formatItemName(name) }}
              </option>
            </select>
          </div>
          <div class="input-field">
            <select v-model="selected_month" class="item-select">
              <option v-for="month in monthOptions" :key="month" :value="month">
                {{ month }}
              </option>
            </select>
          </div>
          <div class="input-field">
            <input type="number" v-model="selected_large_number" placeholder="Crate Number" class="item-input-number">
          </div>
          <div class="input-field">
            <input v-model="name" placeholder="Name" class="item-name">
          </div>
          <div v-for="(lore, index) in lores" :key="index" class="textarea-field inline-field">
            <textarea v-model="lores[index]" placeholder="Lore" class="item-lore"></textarea>
            <button v-if="index === lores.length - 1" class="add-button" @click="addLoreField">+</button>
          </div>
          <button class="generate-button" @click="fillTextBoxes">Generate</button>
        </div>
      </div>
      <div class="right-side" v-show="open">
        <div class="bordered-textarea align-center">
          <div class="textarea-field">
            <textarea v-model="textBox1" disabled placeholder="Item Collector Info" class="readonly-item-lore"></textarea>
          </div>
          <div class="textarea-field">
            <textarea v-model="textBox2" disabled placeholder="CrazyCrates Info" class="readonly-item-lore"></textarea>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.item-creator {
  display: flex;
  flex-direction: column;
  gap: 20px;
  width: 100%;
  padding-bottom: 20px; /* Add extra spacing at the bottom */
}

.top-bar {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 100%;
}

.flex-container {
  display: flex;
  justify-content: flex-end;
}

.inline-field {
  display: flex;
  align-items: start;
}

.bordered-text {
  border: 1px solid #000;
  padding: 5px;
}

/* New class to give a border to div containing textareas */
.bordered-textarea {
  border: 2px solid #000;
  padding: 15px;
  margin-bottom: 15px;
}

.add-button {
  margin-left: 10px;
}

.item-input-number {
  margin: 20px 0;
  padding: 10px;
  background-color: #ccc;
  cursor: pointer;
}

.content {
  font-size: 18px;
  border: 1px solid #ccc;
  border-radius: 3px;
}

.readonly-item-lore {
  font-size: 16px;
  padding: 10px;
  margin-top: 20px;
  width: 100%;
  max-width: 500px;
  border: 1px solid #000;
  border-radius: 4px;
  height: 100px;
  background: #eee;
}
</style>
