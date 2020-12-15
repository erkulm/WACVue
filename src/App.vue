<template>
  <div id="app">
    <div class="app-container" style="background-color: black">
      <HelloWorld msg="Welcome to Web Accessibility App" />
      <form @submit.prevent="greet">
        <InputText type="text" v-model="text" />
        <Button type="submit" label="Search" />
        <h3>{{ message }}</h3>
      </form>

      <div style="display: inline-block; margin-top: 25px;">
        <div style="position: absolute; max-width: 500px;">
          <ErrorTable> </ErrorTable>
        </div>

        <div id="sourcepage" style="margin-left: 525px; max-width: 800px"> 
          <iframe id="my_frame"  :src="source"></iframe>
        </div>
      </div>

    <div style="display : none">
              <Error/>
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
      text: null,
      columns: null,
      errors: null,
      source: "http://localhost:8086/iframe-content?address=https://en.wikipedia.org/wiki/Death_of_Cleopatra",
    };
  },
  created() {
    this.columns = [
      { field: "errorScene", header: "Scene" },
      { field: "document", header: "Document" },
      { field: "testCrDate", header: "Date" },
      { field: "errorDesc", header: "Description" },
    ];
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
      });

    // this.productService.getPoductsSmall().then(data => this.products = data);
  },
  methods: {
    greet() {
      this.$toast.add({ severity: "info", summary: "Hello " + this.text });
      this.message = "Hello " + this.text;
    },
  },
  components: {
    HelloWorld,
    ErrorTable,
    Error
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