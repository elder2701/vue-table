<template>
  <div class="table-container">
    <div class="add-row-container">
      <button class="add-row-button" @click="$emit('on-open-dialog', -1)">
        Add row
      </button>
    </div>

    <div class="table">
      <table id="my-table">
        <tr>
          <th v-for="text in configHeader" :key="text">{{ text }}</th>
          <th />
          <th />
        </tr>
        <MyTableRows
          v-for="item in dataTable"
          :key="item.id"
          v-bind:rowData="item"
          @on-open-dialog="$emit('on-open-dialog', $event)"
          @on-delete="$emit('on-delete', $event)"
        />
      </table>
    </div>
  </div>
</template>

<script>
import MyTableRows from "./MyTableRows.vue";
const configHeader = [
  "Prodact Name",
  "Unit Price",
  "Units in Stock",
  "Discontinued",
];
export default {
  name: "MyTable",
  data: function() {
    return { configHeader };
  },
  props: {
    dataTable: Array,
  },
  components: {
    MyTableRows,
  },
};
</script>

<style scoped>
.add-row-container {
  border: 1px solid rgba(204, 204, 204, 0.664);
  background-color:  rgba(241, 238, 238, 0.979);
}

table {
  border: 1px solid rgba(204, 204, 204, 0.664);
  border-collapse: collapse;
  text-align: center;
}

.table {
  height: 400px;
  overflow: auto;
  overflow-y:scroll
}


th {
  padding: 10px;
  border: 1px solid rgba(204, 204, 204, 0.664);
}

tr {
  border: 1px solid rgba(204, 204, 204, 0.664);
}

.add-row-button {
  margin: 10px;
}

</style>
