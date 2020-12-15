<template>
  <DataTable
    class="p-datatable-sm"
    :value="groupedErrors"
    v-model:expandedRows="expandedRows"
    dataKey="document"
  >
    <template #header>
      <div class="table-header-container">
        <Button
          icon="pi pi-plus"
          label="Expand All"
          @click="expandAll"
          class="p-mr-2"
        />
        <Button icon="pi pi-minus" label="Collapse All" @click="collapseAll" />
      </div>
    </template>
    <Column :expander="true" headerStyle="width: 3rem" />
    <Column field="document" sortable></Column>
    <template #expansion="slotProps">
      <div class="orders-subtable">
        <h5>Errors for {{ slotProps.data.document }}</h5>
        <DataTable :value="slotProps.data.details">
          <Column field="errorDesc" header="Desc" sortable></Column>
          <Column field="testCrDate" header="Date" sortable></Column>
          <template #body class="area">
            <Column field="errorAddress"></Column>
          </template>
          <Column headerStyle="width:4rem" field="errorScene">
            <template #body="slotProps">
              <Button
                icon="pi pi-search"
                @click="showError(slotProps.data.id, $event)"
              />
            </template>
          </Column>
        </DataTable>
      </div>
    </template>
  </DataTable>
</template>

<script>
import DataTable from "primevue/datatable";
import Column from "primevue/column";
import Button from "primevue/button";

export default {
  data() {
    return {
      errors: null,
      expandedRows: [],
      groupedErrors: [],
    };
  },
  mounted() {
    fetch(
      "http://localhost:8086/generate-report?address=https://en.wikipedia.org/wiki/Death_of_Cleopatra",
      {
        method: "get",
      }
    )
      .then((response) => {
        return response.json();
      })
      .then((jsonData) => {
        this.errors = jsonData;
        this.groupedErrors = groupBy(this.errors);
      });
  },
  components: {
    DataTable,
    Column,
    Button,
  },
  methods: {
    showError(details) {
      let iframe = document.getElementById('my_frame');
      iframe.contentWindow.postMessage(details,'*');
    },
    expandAll() {
      this.expandedRows = this.errors.filter((p) => p.document);
      this.$toast.add({
        severity: "success",
        summary: "All Rows Expanded",
        life: 3000,
      });
    },
    collapseAll() {
      this.expandedRows = null;
      this.$toast.add({
        severity: "success",
        summary: "All Rows Collapsed",
        life: 3000,
      });
    },
  },
};

function groupBy(list) {
  var groupedErrors = {};
  for (var i = 0; i < list.length; i++) {
    var nameOfGroup = list[i].document;
    if (!groupedErrors[nameOfGroup]) {
      groupedErrors[nameOfGroup] = [];
    }
    let detailsObject = {};
    detailsObject.id = list[i].id;
    detailsObject.errorDesc = list[i].errorDesc;
    detailsObject.testCrDate = list[i].testCrDate;
    detailsObject.errorScene = list[i].errorScene;
    detailsObject.errorAddress = list[i].errorAddress;
    groupedErrors[nameOfGroup].push(detailsObject);
  }
  list = [];
  for (var gName in groupedErrors) {
    list.push({ document: gName, details: groupedErrors[gName] });
  }
  return list;
}
</script>

<style scoped>
</style>