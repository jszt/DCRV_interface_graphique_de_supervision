<template>
<section class="section">
    <div class="container control-container">
        <button class="button" v-on:click="connexion">Se Connecter</button>
        <button class="button" v-on:click="deconnexion">Se Déconnecter</button>
    </div>
    <div class="container control-container">
        <button class="button" v-on:click="drapeau_vert"><img class="image is-24x24" src="../assets/drapeau_vert.png">Drapeau vert</button>
        <button class="button" v-on:click="drapeau_rouge"><img class="image is-24x24" src="../assets/drapeau_rouge.png">Drapeau rouge</button>
        <button class="button" v-on:click="drapeau_jaune"><img class="image is-24x24" src="../assets/drapeau_jaune.png">Drapeau Jaune</button>
        <button class="button" v-on:click="drapeau_noir"><img class="image is-24x24" src="../assets/drapeau_noir.png">Drapeau noir</button>
        <button class="button" v-on:click="drapeau_noir_damier"><img class="image is-24x24" src="../assets/drapeau_noir_damier.png">Drapeau Noir à Damier</button>
    </div>
</section>
</template>

<script>
export default {
    name: 'ControlComponent',
    data() {
        return {
          // Variable contenant le socket 
          websocket: null
        }
    },
    methods: {
        // La méthode connexion permet d'ouvrir une connexion Web socket et d'écouter les connexions entrantes venant des voitures
        connexion() {
            // On ouvre une connexion web socket avec le paramètre interface qui correspond à l'id de ce client
            let socket = new WebSocket("ws://localhost:8000/interface")
            // On stocke le socket
            this.websocket = socket

            // Evenement lors de la connexion au serveur
            socket.onopen = function (event) {
                alert('connexion au serveur effectuée')
            }

            // Evenement lors de la réception d'un message de la part
            socket.onmessage = function (event) {
              // On envoie la position au composant parent, qui va l'envoyer au displayComponent
              this.$emit('position', event.data)
            }.bind(this)

            // Evenement lors de la fermeture de la socket
            socket.onclose = function (event) {
                socket.close()
            }
        },
        deconnexion() {
            this.websocket.close()
        },
        drapeau_vert() {
          let json_drapeau_vert = {
            "mode": "DRAPEAU_VERT"
          }
          this.websocket.send(JSON.stringify(json_drapeau_vert))
        },
        drapeau_rouge() {
          let json_drapeau_rouge = {
            "mode": "DRAPEAU_ROUGE",
            "vitesse": 0
          }
          this.websocket.send(JSON.stringify(json_drapeau_rouge))
        },
        drapeau_jaune() {
          let json_drapeau_jaune = {
            "mode": "DRAPEAU_JAUNE"
          }
          this.websocket.send(JSON.stringify(json_drapeau_jaune))
        },
        drapeau_noir() {
          let json_drapeau_noir = {
            "mode": "DRAPEAU_NOIR"
          }
          this.websocket.send(JSON.stringify(json_drapeau_noir))
        },
        drapeau_noir_damier() {
          let json_drapeau_noir_damier = {
            "mode": "DRAPEAU_NOIR_A_DAMIER"
          }
          this.websocket.send(JSON.stringify(json_drapeau_noir_damier))
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
.control-container {
    text-align: center;
    margin-bottom: 3%;
}

button {
    margin: 0.5%;
}
</style>
