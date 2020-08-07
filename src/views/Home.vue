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
import Handsontable from "handsontable";
// import Handsontable from "handsontable";

export default {
  name: "Home",
  data: function() {
    return {
      colHeaders: ["State", "Task", "Assigned To", "Reviewed", "Status", "Due"],
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
      // let hotData = Array.from({
      //   length: 23 - this.baseData.length
      // }).map(() => []);
      // hotData.unshift(...this.baseData);
      return {
        data: this.baseData,
        minCols: 26,
        minRows: 26,
        width: "100vw",
        height: "100vh",
        contextMenu: true,
        className: "htLeft",
        minSpareCols: 3,
        minSpareRows: 2,
        // cell: [{ col: 0, className: "htCenter" }],
        columns: [
          {
            className: "htCenter",
            renderer: function(instance, TD, row) {
              let data = instance.getDataAtRow(row);
              let parent = document.createElement("div");
              let div = document.createElement("div");
              let colorMap = {
                "In Progress": "yellow",
                Completed: "green",
                "Not Started": "red"
              };

              parent.style = `display:flex;justify-content:center;align-items:center`;
              div.style = `background-color: ${
                colorMap[data[4]]
              };width:20px;height:20px;border-radius:50%;`;

              parent.appendChild(div);
              Handsontable.dom.empty(TD);
              TD.appendChild(parent);

              return TD;
            }
          },
          {},
          {},
          { type: "checkbox" },
          {
            editor: "select",
            selectOptions: ["In Progress", "Not Started", "Completed"],
            width: 120
          },
          { type: "date", width: 120 }
          // ...Array.from({ length: 20 }).map(() => {})
        ],
        colHeaders: this.colHeaders
      };
    }
  },
  created() {
    this.hotSettings = this.settings;
  }
};
</script>

<style src="../../node_modules/handsontable/dist/handsontable.full.css"></style>
