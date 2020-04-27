<template>
  <tr>
    <td>{{rowData.product}}</td>
    <td>{{rowData.price}}</td>
    <td>{{rowData.units}}</td>
    <td>{{rowData.discontinued}}</td>
    <td class="btn-control">
      <button class="update-button" @click="$emit('on-open-dialog', rowData.id)">Edit</button>
    </td>
    <td><button class="cancel-button" @click="$emit('on-delete', rowData.id)">Delete</button></td>
  </tr>
</template>

<script>
export default {
  name: "MyTableRows",
  props: { rowData: Object }
};
</script>

<style>
th, td {
  border: 1px solid rgba(204, 204, 204, 0.664);
}
</style>