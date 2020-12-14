<template>
<div id="main">
    <control-component @position="getPosition" />
    <display-component v-bind:position="position"/>
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
          position: 0
        }
    },
    methods: {
      getPosition(position){
        this.position = position;
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
