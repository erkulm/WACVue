<template>
  <DataTable :value="errors" class="p-datatable-striped" :autoLayout="true">
    <Column
      v-for="col of columns"
      :field="col.field"
      :header="col.header"
      :key="col.field"
    ></Column>
  </DataTable>
</template>

<script>
import DataTable from "primevue/datatable";
import Column from "primevue/column";

export default {
  data() {
    return {
      columns: null,
      errors: null,
    };
  },
  productService: null,
  components: {
    DataTable,
    Column,
  },
  created() {
    this.columns = [
      { field: "errorScene", header: "Scene" },
      { field: "document", header: "Document" },
      { field: "testCrDate", header: "Date" },
      { field: "errorAddress", header: "Address" },
    ];
  },
  mounted() {
    fetch(
      "http://localhost:8086/generate-report?address=https://en.wikipedia.org/wiki/OpenCV",
      {
        method: "get",
      }
    )
      .then((response) => {
        return response.json();
      })
      .then((jsonData) => {
        this.errors = jsonData;
      });

    // this.productService.getProductsSmall().then(data => this.products = data);
  },
};
</script>

