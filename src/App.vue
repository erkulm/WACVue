<template>
  <div id="app">
    <div class="app-container" style="background-color: black">
      <HelloWorld msg="Welcome to Web Accessibility App" />
      <form @submit.prevent="greet">
        <InputText type="text" v-model="address" />
        <Button type="submit" label="Search" @click="createDataTable()" />
        <h3>{{ address }}</h3>
      </form>

      <div
        v-if="this.address != null"
        style="display: inline-block; margin-top: 25px"
      >
        <div style="position: absolute; max-width: 500px">
          <ErrorTable :address="address"> </ErrorTable>
        </div>

        <div
          v-if="this.source != null"
          id="sourcepage"
          style="margin-left: 525px; max-width: 800px"
        >
          <iframe id="my_frame" :src="this.source"></iframe>
        </div>
      </div>

      <div style="display: none">
        <Error />
      </div>
    </div>

    <Toast />
  </div>
</template>
<script>
import HelloWorld from "./components/HelloWorld.vue";
import ErrorTable from "./components/ErrorTable.vue";
import Error from "./components/Error.vue";

export default {
  data() {
    return {
      message: null,
      address: null,
      columns: null,
      errors: null,
      source: null,
    };
  },
  //computed(){
  //    source: "http://localhost:8086/iframe-content?address=" + this.address
  //},
  created() {
    this.columns = [
      { field: "errorScene", header: "Scene" },
      { field: "document", header: "Document" },
      { field: "testCrDate", header: "Date" },
      { field: "errorDesc", header: "Description" },
    ];
  },

  methods: {
    greet() {
      this.$toast.add({ severity: "info", summary: "Hello " + this.text });
      this.message = "Hello " + this.text;
    },
    createDataTable() {
      this.source = "http://localhost:8086/iframe-content?address=" + this.address,
        fetch(this.source, {
          method: "get",
        })
          .then((response) => {
            return response.json();
          })
          .then((jsonData) => {
            this.errors = jsonData;
          });
    },
  },
  components: {
    HelloWorld,
    ErrorTable,
    Error,
  },
};
</script>

<style scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

#sourcepage,
#sourcepage iframe {
  height: 100vh;
  width: 55vw;
  border: none;
  box-sizing: border-box;
}
body {
  margin: 0;
}

.app-container {
  text-align: center;
}

body #app .p-button {
  margin-left: 0.2em;
}

form {
  margin-top: 2em;
}
</style>