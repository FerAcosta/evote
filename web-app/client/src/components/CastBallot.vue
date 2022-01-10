<template>
  <div class="posts">
    <h1>Emite tu Voto</h1>
    <br>
    <input type="radio" id="one" value="Republican" v-model="picked">
    <label for="one">Tobey Maguire</label>
    <br>
    <input type="radio" id="two" value="Democrat" v-model="picked">
    <label for="two">Andrew Garfield</label>
    <br>
    <input type="radio" id="two" value="Green" v-model="picked">
    <label for="two">Tom Holland</label>
    <br>
    <br>
    <!--span><b>{{ response }}</b></span><br /-->
    <form v-on:submit="castBallot">
      <!-- <input type="text" value="2sww593dc034wb2twdk91r" v-model="input.electionId"  >
      <br>-->
      <input type="text" v-model="input.voterId" placeholder="Número de Cuenta">
      <br>
      <input type="submit" value="Emitir Voto">
      <br>
    </form>

    <br>
    <span v-if="response">
      <b>{{ response }}</b>
    </span>
    <br>
    <vue-instant-loading-spinner id="loader" ref="Spinner"></vue-instant-loading-spinner>
  </div>
</template>

<script>
import PostsService from "@/services/apiService";
import VueInstantLoadingSpinner from "vue-instant-loading-spinner/src/components/VueInstantLoadingSpinner.vue";

export default {
  name: "response",
  data() {
    return {
      input: {},
      picked: null,
      response: null
    };
  },
  components: {
    VueInstantLoadingSpinner
  },
  methods: {
    async castBallot() {
      await this.runSpinner();

      const electionRes = await PostsService.queryWithQueryString('election');

      let electionId = electionRes.data[0].Key;

      console.log("picked: ");
      console.log(this.picked);
      console.log("voterId: ");
      console.log(this.input.voterId);
      this.response = null;

 

      //error checking for making sure to vote for a valid party
      if (this.picked === null ) {
        console.log('this.picked === null')

        let response = "¡Debes elegir una opción para votar!";
        this.response = response;
        await this.hideSpinner();
      
      } else if (this.input.voterId === undefined) {
        console.log('this.voterId === undefined')

        let response = "Ingresa tu número de cuenta para votar.";
        this.response = response;
        await this.hideSpinner();

      } else {

        const apiResponse = await PostsService.castBallot(
          electionId,
          this.input.voterId,
          this.picked
        );

        console.log('apiResponse: &&&&&&&&&&&&&&&&&&&&&&&');
        console.log(apiResponse);

        if (apiResponse.data.error) {
          this.response= apiResponse.data.error;
          await this.hideSpinner();
        } else if (apiResponse.data.message) {
          this.response= `No se pudo encontrar el número de usuario ${this.input.voterId}
            en el sistema. Asegurate de ingresar un número de usuario válido.`;
          await this.hideSpinner();
        } 
        else {
          let response = `Su voto se realizó con éxito 
            con el número de usuario ${apiResponse.data.voterId}.
            Puedes seguir los resultados actualizados de la votación en la pestaña "Resultados de Votación".     ¡Gracias por participar en
            en esta votación!`;

          this.response = response;

          console.log("cast ballot");
          console.log(this.input);
          await this.hideSpinner();
        }
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