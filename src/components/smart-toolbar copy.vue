<template><!-- FIRST ROW: Main entries -->
  <div :style="'cssStyle'" class="flex items-center text-xl">
    
    <div class="grow">
      <!-- <div class="flex justify-start items-center text-gray-500">
        
        <button v-if="showMenu" v-for="item in selectedItems" :key="item.label" :in:scale="{ duration: 250 }" class="ml-3"
          @click="jumpIn(item)" v-else-if="item.input == null">
          <i v-if="item.icon" :class="item.icon" />
          <span v-if="item.label && showLabels">
            {{ item.label }}
          </span>
        </button>
        <input v-else-if="item.input?.type === 'date'" type="date" />
        <input v-else-if="item.input?.type === 'text'" type="text" />
        <select v-else-if="item.input?.type === 'select'" @change="($event) => forwardValue($event, item.input?.update)"
          :value="item.input.bindValueKey ? boundValues[item.input.bindValueKey] : undefined">
          <option v-if="item.input.options" v-for="opt in item.input.options" :key="opt.value" :value="opt.value">{{
            opt.label }}</option>
        </select>
        <input v-else-if="item.input?.type === 'checkbox'" type="checkbox" />
        <input v-else-if="item.input?.type === 'radio'" type="radio" />
      </div> -->
    </div>
  </div>


  <!-- <div v-if="selectedSubItems" class="grid grid-cols-1 bg-white">
    <div v-for="subItem in selectedSubItems" :key="subItem.name" class="flex justify-start items-center p-2 text-xl">
      
      <button v-if="subItem.input == null" in:slide={{ duration: 500 }} @click="jumpIn(subItem)">

        <i v-if="subItem.icon" class={subItem.icon} />
        <span v-if="subItem.label">{subItem.label}</span>

      </button>
      <div v-else>

        
        <i v-if="subItem.icon" :class="`${subItem.icon} mr-2 flex-none`" @click="showAllHints"
          @keypress.enter="showAllHints" tabindex="0"></i>

      
        <span v-if="showHints && !showSubLabels"
          class="fixed bg-slate-200 left-8 text-lg pt-1 pb-1 pl-2 pr-2 rounded-md flex items-center">
          <svg width="24" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill-rule="evenodd" clip-rule="evenodd"
            stroke-linejoin="round" stroke-miterlimit="2">
            <path
              d="M13.789 7.155c.141-.108.3-.157.456-.157.389 0 .755.306.755.749v8.501c0 .445-.367.75-.755.75-.157 0-.316-.05-.457-.159-1.554-1.203-4.199-3.252-5.498-4.258-.184-.142-.29-.36-.29-.592 0-.23.107-.449.291-.591 1.299-1.002 3.945-3.044 5.498-4.243z" />
          </svg>
          {{ subItem.label }}
        </span>

       
        <span v-if="subItem.label && showSubLabels" class="mr-2 flex-none">{{ subItem.label }}</span>

       
        <div class="grow">
          <input v-if="subItem.input.type === 'text'" class="w-full" type="text"
            :placeholder="subItem.input.placeholder || ''"
            :value="subItem.input.bindValueKey ? boundValues[subItem.input.bindValueKey] : undefined"
            @input="($event) => forwardValue($event, subItem.input?.update)" />

          <input v-if="subItem.input.type === 'date'" class="w-full" type="date"
            :value="subItem.input.bindValueKey ? boundValues[subItem.input.bindValueKey] : undefined"
            @change="($event) => forwardValue($event, subItem.input?.update)" />

          <input v-if="subItem.input.type === 'datetime-local'" class="w-full" type="datetime-local"
            :value="subItem.input.bindValueKey ? boundValues[subItem.input.bindValueKey] : undefined"
            @change="($event) => forwardValue($event, subItem.input?.update)" />

          <select v-if="subItem.input.type === 'select'" class="w-full"
            :value="subItem.input.bindValueKey ? boundValues[subItem.input.bindValueKey] : undefined"
            @change="($event) => forwardValue($event, subItem.input?.update)">
            <template v-if="subItem.input.options">
              <option v-for="(opt, index) in subItem.input.options" :key="index" :value="opt.value">{{ opt.label }}
              </option>
            </template>
          </select>

          <input v-if="subItem.input.type === 'checkbox'" class="w-full" type="checkbox"
            :value="subItem.input.bindValueKey ? boundValues[subItem.input.bindValueKey] : undefined"
            @change="($event) => forwardValue($event, subItem.input?.update)" />

          <input v-if="subItem.input.type === 'radio'" class="w-full" type="radio" />


        </div>

      </div>

    </div>
  </div> -->


  <!-- Slot After -->
<!-- <div class="flex-none w-14">
    <slot name="after" />
  </div> --></template>




<script setup lang="ts">

import { computed, ref, watch } from 'vue'
import type { MenuItem } from "./interface";

const props = defineProps<{
  showLabels: boolean,
  showSubLabels: boolean,
  items: MenuItem[],
  style: any,
  boundValues: any,
}>()

const cssStyle = computed(() => {
  Object.entries(props.style)
    .map(([key, value]) => `${key}:${value}`)
    .join(";");
});

// entries for menu
// visible top level entries
let selectedItems: MenuItem[] = props.items;
// selected mene item entry
let selectedEntry: null | MenuItem = null;
// selected menu item submenu
let selectedSubItems: null | MenuItem[] = null;
// active entry name
let activeEntry: null | string = null;

// start entry
const updateItemsFromParent = (e: MenuItem[]) => {
  console.log("updateItemsFromParent", e);
  selectedItems = e;
};

watch(props.items, (e) => {
  updateItemsFromParent(e);
});

// select an menu item
const jumpIn = (item: MenuItem) => {
  if (item.name === activeEntry) {
    selectedSubItems = null;
    selectedSubItems = null;
  } else {
    if (item.items) {
      selectedEntry = item;
      selectedItems = item.items;
      selectedSubItems = null;
      activeEntry = item.name;
    } else if (item.action) {
      item.action(item.name);
    } else if (item.subMenuItems) {
      activeEntry = item.name;
      selectedSubItems = item.subMenuItems;
    }
  }
};

// go back to previous menu
const jumpOut = () => {
  selectedEntry = null;
  selectedSubItems = null;
  selectedItems = props.items;
};

// pause 500ms before showing menu
let showMenu: boolean = false;
setTimeout(() => {
  showMenu = true;
}, 500);

let showHints: boolean = false;
const showAllHints = () => {
  showHints = true;
  setTimeout(() => {
    showHints = false;
  }, 2000);
};

const forwardValue = (event: any, updateFunc: any) => {
  if (event.target.value) {
    updateFunc(event.target.value);
  }
};

</script>