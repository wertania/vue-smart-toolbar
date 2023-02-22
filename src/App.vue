<template>
  <div style="margin: 0 auto; width: 800px">
    <SmartToolbar :items="menu" :show-labels="true" :show-sub-labels="true" :bound-values="bindTest" :style="style">
      <template v-slot:before>
        test
      </template>
    </SmartToolbar>
  </div>
</template>

<script setup lang="ts">
import { MenuItem } from './components/interface';
import SmartToolbar from './components/smart-toolbar.vue';

const style = {
  "background-color": "white",
  height: "50px",
  width: "100%",
}

let bindTest = {
  interval: "P1W",
  test2: "abc",
  start: "2021-12-31T23:00:00",
  timespan: "P1D",
  chartType: "line",
};

const menu: MenuItem[] = [
  // chart settings
  {
    name: "chart-settings",
    icon: "fa fa-clock",
    subMenuItems: [
      {
        name: "timespan",
        icon: "fa fa-calendar",
        label: "Timespan",
        input: {
          type: "select",
          options: [
            {
              label: "1 day",
              value: "P1D",
            },
            {
              label: "1 week",
              value: "P1W",
            },
            {
              label: "1 month",
              value: "P1M",
            },
            {
              label: "1 year",
              value: "P1Y",
            },
          ],
          update(val) {
            bindTest.timespan = val;
          },
          bindValueKey: "timespan",
        },
      },
      {
        name: "interval",
        icon: "fa fa-clock",
        label: "Interval",
        input: {
          type: "select",
          options: [
            {
              label: "15min",
              value: "PT15M",
            },
            {
              label: "1 day",
              value: "P1D",
            },
            {
              label: "1 week",
              value: "P1W",
            },
            {
              label: "1 month",
              value: "P1M",
            },
            {
              label: "1 year",
              value: "P1Y",
            },
          ],
          update(val) {
            bindTest.interval = val;
          },
          bindValueKey: "interval",
        },
      },
    ],
  },
  // times
  {
    name: "times",
    icon: "fa fa-arrows-left-right-to-line",
    subMenuItems: [
      {
        name: "start-ts",
        label: "Start Timestamp",
        icon: "fa fa-arrow-right-from-bracket",
        input: {
          type: "datetime-local",
          update(val) {
            bindTest.start = val;
          },
          bindValueKey: "start",
        },
      },
      {
        name: "end-ts",
        label: "End Timestamp",
        icon: "fa fa-arrow-right-to-bracket",
        input: {
          type: "datetime-local",
          update(val) {
            console.log(val);
          },
        },
      },
    ],
  },
  // move timestamps
  {
    name: "move-previous",
    icon: "fa fa-chevron-left",
    action: (name) => {
      console.log(name);
    },
  },
  {
    name: "move-next",
    icon: "fa fa-chevron-right",
    action: (name) => {
      console.log(name);
    },
  },
  {
    name: "chart-type",
    icon: "fa fa-chart-line",
    items: [
      {
        name: "input-chart-type",
        input: {
          type: "select",
          options: [
            {
              label: "Line",
              value: "line",
            },
            {
              label: "Bar",
              value: "bar",
            },
            {
              label: "Area",
              value: "area",
            },
          ],
          update(val) {
            bindTest.chartType = val;
          },
          bindValueKey: "chartType",
        },
      },
    ],
  },
  {
    name: "share",
    icon: "fa fa-share",
    subMenuItems: [
      {
        name: "export-url",
        icon: "fa fa-link",
        label: "Export URL",
        action: (name) => {
          console.log("send link");
        },
      },
    ],
  },
];
</script>

<style>
@import "./../node_modules/@fortawesome/fontawesome-free/css/all.min.css";

body {
  background-color: rgb(229, 229, 229);
}
</style>