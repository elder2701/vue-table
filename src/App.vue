<template>
  <div id="app">
    <MyTable
      v-bind:dataTable="dataTable"
      @on-open-dialog="handleOpenDialog"
      @on-delete="handleDeleteRow"
    />
    <ProductDialog
      v-show="showDialog"
      @on-close="handleCloseDialog"
      @on-update-data="handleUpdate"
      v-bind:selectedRow="getFullSelectedRow()"
    />
  </div>
</template>

<script>
import MyTable from "./components/MyTable.vue";
import ProductDialog from "./components/ProductDialog.vue";
import dataTable from "./mock";

export default {
  name: "App",
  data: function() {
    return { showDialog: false, dataTable, selectedRowId: -1 };
  },
  methods: {
    handleOpenDialog(selectedRowId) {
      this.showDialog = true;
      if (selectedRowId !== -1) {
        this.selectedRowId = selectedRowId;
      }
    },
    handleCloseDialog() {
      this.showDialog = false;
      this.selectedRowId = -1;
    },
    handleDeleteRow: function(idToRemove) {
      this.dataTable = this.dataTable.filter(row => {
        return row.id !== idToRemove;
      });
    },
    handleUpdate: function(dataToUpdate) {
      this.showDialog = false;
      console.log(this.selectedRowId);
      if (this.selectedRowId === -1) {
        const id = Date.now();
        this.dataTable.push({ id, ...dataToUpdate.newData });
      } else {
        this.dataTable = this.dataTable.map(item => {
          return item.id === this.selectedRowId
            ? { id: item.id, ...dataToUpdate.newData }
            : item;
        });
        this.selectedRowId = -1;
      }
    },
    getFullSelectedRow: function() {
      if (this.selectedRowId === -1) {
        return {};
      } else {
        return this.dataTable.find(item => this.selectedRowId === item.id);
      }
    }
  },
  components: {
    MyTable,
    ProductDialog
  }
};
</script>

<style>
#app {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
