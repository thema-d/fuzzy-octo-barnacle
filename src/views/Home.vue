<template>
  <hot-table
    licenseKey="non-commercial-and-evaluation"
    :key="'sprout_the_most_awesome_ljakldjaklfjd'"
    :id="'main_hot_table'"
    :settings="hotSettings"
    :rowHeaders="true"
    :colHeaders="true"
    ref="hotTable"
  ></hot-table>
</template>

<script>
import { HotTable } from "@handsontable/vue";
import Handsontable from "handsontable";

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
      hotSettings: {},
      savedDataFromLocalStorage: {}
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
        // minCols: 26,
        minRows: 23,
        width: "100vw",
        height: "100vh",
        contextMenu: true,
        className: "htLeft",
        // minSpareCols: 1,
        minSpareRows: 2,
        manualRowMove: true,
        manualColumnMove: true,
        allowEmpty: true,
        cells: function(row, col) {
          if (col == 0) {
            this.readOnly = true;
            this.renderer = function(instance, TD, row) {
              let data = instance.getDataAtRow(row);
              let parent = document.createElement("div");
              let div = document.createElement("div");
              let indexOfStatus = instance.getColHeader().indexOf("Status");
              let colorMap = {
                "In Progress": "yellow",
                Completed: "green",
                "Not Started": "red"
              };

              parent.style = `display:flex;justify-content:center;align-items:center`;
              div.style = `background-color: ${
                colorMap[data[indexOfStatus]]
              };width:20px;height:20px;border-radius:50%;`;

              parent.appendChild(div);
              Handsontable.dom.empty(TD);
              TD.appendChild(parent);

              return TD;
            };
          } else if (col == 3) {
            this.type = "checkbox";
            this.className = "htCenter";
          } else if (col == 4) {
            this.editor = "select";
            this.selectOptions = ["In Progress", "Not Started", "Completed"];
            this.width = 120;
          } else if (col == 5) {
            this.type = "date";
          }
        },
        colHeaders: this.colHeaders,
        persistentState: true,
        afterChange: (changes, source) => {
          if (source == "loadData") {
            this.$refs.hotTable.hotInstance.runHooks(
              "persistentStateLoad",
              "task_force",
              this.savedDataFromLocalStorage
            );
            if (this.savedDataFromLocalStorage.value) {
              this.$refs.hotTable.hotInstance.loadData(
                this.savedDataFromLocalStorage.value
              );
            }
          } else {
            let data = this.$refs.hotTable.hotInstance.getData();
            this.$refs.hotTable.hotInstance.runHooks(
              "persistentStateSave",
              "task_force",
              data
            );
          }
        }
      };
    }
  },
  created() {
    this.hotSettings = this.settings;
  }
};
</script>

<style src="../../node_modules/handsontable/dist/handsontable.full.css"></style>
