<template>
<div id="main">
    <control-component @voiture="getEtatVoiture" />
    <display-component v-bind:voiture="voiture"/>
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
          voiture: null
        }
    },
    methods: {
      getEtatVoiture(voiture){
        this.voiture = voiture;
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
