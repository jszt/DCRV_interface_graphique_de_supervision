<template>
<div id="main">
    <display-component 
      v-bind:voiture="voiture" 
      v-bind:state="state"
      v-bind:websocket="websocket"
    />
    <control-component 
      @newdatafromcar="getCarData" 
      @changestate="changeState"
      @websocket="getWebsocket"
    />
</div>
</template>

<script>
import ControlComponent from './ControlComponent.vue'
import DisplayComponent from './DisplayComponent.vue'

export default {
    name: 'MainComponent',
    components: {
        ControlComponent,
        DisplayComponent
    },
    data() {
        return {
          voiture: null,
          // Indique si l'on est connecté ou pas, de base non
          state: false,
          // Gère la communication avec le serveur
          websocket: null
        }
    },
    methods: {
      getCarData(data){
        this.voiture = data;
      },
      changeState(state){
        // On envoie le nouvel état au DisplayComponent
        this.state = state;
      },
      getWebsocket(websocket){
        // On récupère le websocket permmettant de communiquer avec le serveur
        this.websocket = websocket
      }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
#main {
    height: 100%;
    width: 100%;
    position: absolute;
    background-color: #2c2f34;
}
</style>
