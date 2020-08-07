<template>
  <hot-table
    licenseKey="non-commercial-and-evaluation"
    :settings="hotSettings"
    :rowHeaders="true"
    :colHeaders="true"
  ></hot-table>
</template>

<script>
import { HotTable } from "@handsontable/vue";
// import Handsontable from "handsontable";

export default {
  name: "Home",
  data: function() {
    return {
      baseData: [
        [
          -1,
          "Make Div Table",
          "Noel",
          true,
          "In Progress",
          new Date().toLocaleDateString()
        ],
        [
          0,
          "Make Insert of the Column",
          "Patrick",
          false,
          "Not Started",
          new Date(2020, 7, 20).toLocaleDateString()
        ],
        [
          1,
          "Make components for every type",
          "John",
          false,
          "Completed",
          new Date().toLocaleDateString()
        ]
      ],
      hotSettings: {}
    };
  },
  components: {
    HotTable
  },
  computed: {
    settings() {
      let hotData = Array.from({
        length: 23 - this.baseData.length
      }).map(() => []);
      hotData.unshift(...this.baseData);
      return {
        data: hotData,
        minCols: 26,
        minRows: 26,
        cellAlignment: "left",
        colWidth: 150,
        columns: [
          {},
          {},
          {},
          { type: "checkbox" },
          {
            editor: "select",
            selectOptions: ["In Progress", "Not Started", "Completed"]
          },
          { type: "date" }
        ],
        colHeaders: [
          "State",
          "Task",
          "Assigned To",
          "Reviewed",
          "Status",
          "Due"
        ]
      };
    }
  },
  created() {
    this.hotSettings = this.settings;
  }
};
</script>

<style src="../../node_modules/handsontable/dist/handsontable.full.css"></style>
