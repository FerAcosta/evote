<template>
  <div class="posts">
    <h1>Consulta por Clave</h1>
    <form v-on:submit="queryByKey">
      <input type="text" v-model="input.key" placeholder="Ingresa Clave">
      <br>

      <input type="submit" value="Consultar">
      <br>
      <br>
      <span v-if="input">
        <b>{{ input.data }}</b>
      </span>  
      <br>
    </form>

    <br>
      <vue-instant-loading-spinner id='loader' ref="Spinner"></vue-instant-loading-spinner>
  </div>
</template>

<script>
import PostsService from "@/services/apiService";
import VueInstantLoadingSpinner from 'vue-instant-loading-spinner/src/components/VueInstantLoadingSpinner.vue'

export default {
  name: "response",
  data() {
    return {
     input: {
        data: ""
      }
    };
  },
  name: 'app',
  components: {
    VueInstantLoadingSpinner
  },
  methods: {
    async queryByKey() {
        this.runSpinner();
      console.log('this.input: ');
      console.log(this.input);
      if (!this.input.key) {
        console.log('this.input$#: ');
        let response = 'Ingresa la clave a consultar.';
        this.input.data = response;
        this.hideSpinner();
      } else {
        this.runSpinner();
        const apiResponse = await PostsService.queryByKey(this.input.key);
        console.log(apiResponse);
        this.input = apiResponse;
        this.hideSpinner();
      }
    },
    async runSpinner() {
      this.$refs.Spinner.show();
    },
    async hideSpinner() {
      this.$refs.Spinner.hide();
    }
  }
};
</script>
