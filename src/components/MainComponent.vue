<template>
<div id="main">
    <control-component @newdatafromcar="getCarData" @changestate="changeState" />
    <display-component v-bind:voiture="voiture" v-bind:state="state"/>
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
          state: false
        }
    },
    methods: {
      getCarData(data){
        this.voiture = data;
      },
      changeState(state){
        // On envoie le nouvel état au DisplayComponent
        this.state = state;
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
