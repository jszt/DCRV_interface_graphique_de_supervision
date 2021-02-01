<template>
<section class="section">
    <div class="container connexion-container">
        <img class="image is-48x48" v-bind:src="isconnecte()" />
        <div class="dropdown is-hoverable">
            <div class="dropdown-trigger">
                <button v-bind:disabled="disabledBouton()" class="button" v-on:click="connexion">
                    Se Connecter
                </button>
            </div>
            <div class="dropdown-menu" id="dropdown-menu4" role="menu">
                <div class="dropdown-content">
                    <div class="dropdown-item">
                        <p>Se connecter permet de démarrer une connexion pour que les voitures puissent se connecter</p>
                    </div>
                </div>
            </div>
        </div>

        <div class="dropdown is-hoverable">
            <div class="dropdown-trigger">
                <button class="button" v-on:click="deconnexion">Se Déconnecter</button>
            </div>
            <div class="dropdown-menu" id="dropdown-menu4" role="menu">
                <div class="dropdown-content">
                    <div class="dropdown-item">
                        <p>Se déconnecter permet d'arrêter la connexion avec le serveur et ainsi empêcher les connexions des voitures</p>
                    </div>
                </div>
            </div>
        </div>

    </div>
    <div class="container control-container">

        <div class="dropdown is-hoverable">
            <div class="dropdown-trigger">
                <button class="button bouton-drapeau" v-on:click="drapeau_vert"><img class="image is-24x24" src="../assets/drapeau_vert.png">Drapeau vert</button>
            </div>
            <div class="dropdown-menu" id="dropdown-menu4" role="menu">
                <div class="dropdown-content">
                    <div class="dropdown-item">
                        <p>Drapeau Vert: mode par défaut</p>
                    </div>
                </div>
            </div>
        </div>

        <div class="dropdown is-hoverable">
            <div class="dropdown-trigger">
                <button class="button bouton-drapeau" v-on:click="drapeau_rouge"><img class="image is-24x24" src="../assets/drapeau_rouge.png">Drapeau rouge</button>
            </div>
            <div class="dropdown-menu" id="dropdown-menu4" role="menu">
                <div class="dropdown-content">
                    <div class="dropdown-item">
                        <p>Drapeau Rouge: permet de stopper les voitures</p>
                    </div>
                </div>
            </div>
        </div>

        <div class="dropdown is-hoverable">
            <div class="dropdown-trigger">
                <button class="button bouton-drapeau" v-on:click="drapeau_jaune"><img class="image is-24x24" src="../assets/drapeau_jaune.png">Drapeau Jaune</button>
                <br>
                <input class="input" type="number" v-model="limite" placeholder="Vitesse limite" min="0" max="1" step="0.01">
            </div>
            <div class="dropdown-menu" id="dropdown-menu4" role="menu">
                <div class="dropdown-content">
                    <div class="dropdown-item">
                        <p>Drapeau Jaune: permet de limiter la vitesse des voitures</p>
                    </div>
                </div>
            </div>
        </div>

        <div class="dropdown is-hoverable">
            <div class="dropdown-trigger">
                <button class="button bouton-drapeau" v-on:click="drapeau_noir"><img class="image is-24x24" src="../assets/drapeau_noir.png">Drapeau noir</button>
            </div>
            <div class="dropdown-menu" id="dropdown-menu4" role="menu">
                <div class="dropdown-content">
                    <div class="dropdown-item">
                        <p>Drapeau Noir: permet de faire retourner les voitures au point de départ</p>
                    </div>
                </div>
            </div>
        </div>

        <div class="dropdown is-hoverable">
            <div class="dropdown-trigger">
                <button class="button bouton-drapeau" v-on:click="drapeau_noir_damier"><img class="image is-24x24" src="../assets/drapeau_noir_damier.png">Drapeau Noir à Damier</button>
                <br>
                <input class="input" type="number" v-model="constante" placeholder="Vitesse constante" min="0" max="1" step="0.01">
            </div>
            <div class="dropdown-menu" id="dropdown-menu4" role="menu">
                <div class="dropdown-content">
                    <div class="dropdown-item">
                        <p>Drapeau Noir à damier: permet de faire rouler les voitures à vitesse constante</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
</template>

<script>
export default {
  name: 'ControlComponent',
  data () {
    return {
      // Variable contenant le socket
      websocket: null,
      // Permet de savoir si on est connecté, non de base
      state: false,
      // Drapeau jaune
      limite: null,
      // Drapeau à damier
      constante: null
    }
  },
  methods: {
    // La méthode connexion permet d'ouvrir une connexion Web socket et d'écouter les connexions entrantes venant des voitures
    connexion () {
      // On ouvre une connexion web socket avec le paramètre interface qui correspond à l'id de ce client
      // Ici on demande une connexion sur l'ip du rpi
      //let socket = new WebSocket('ws://10.3.141.1:8000/interface')
       let socket = new WebSocket('ws://localhost:8000/interface')
      // On stocke le socket
      this.websocket = socket

      // On transmet le websocket aux autres composants
      this.$emit('websocket', this.websocket)

      // Evenement lors de la connexion au serveur
      socket.onopen = function (event) {
        alert('Connexion au serveur effectuée')
      }

      // Evenement lors de la réception d'un message de la part
      socket.onmessage = function (event) {
        // On envoie les données reçues de la voiture au composant parent soit MainComponent, qui va l'envoyer au displayComponent
        this.$emit('newdatafromcar', JSON.parse(event.data))
      }.bind(this)

      // Evenement lors de la fermeture de la socket
      socket.onclose = function (event) {
        socket.close()
      }

      // On met l'état à vrai, on indique au Display component que l'on est connecté
      this.$emit('changestate', true)
      this.state = true
    },
    deconnexion () {
      // On réinitialise les apparences
      let boutons = document.getElementsByClassName('bouton-drapeau')
      for (let bouton of boutons) {
        bouton.classList.remove('is-info')
      }
      // On indique au serveur que l'on se déconnecte
      let jsonDeconnexion = {
        'mode': 'DECONNEXION'
      }
      this.websocket.send(JSON.stringify(jsonDeconnexion))
      this.websocket.close()
      // on indique au DisplayComponent que l'on se déconnecte
      this.$emit('changestate', false)
      this.state = false
    },
    // Ces fonctions sont déclenchées au clique d'un bouton d'un drapeau et envoyent l'information du choix du mode au serveur
    drapeau_vert () {
      this.activeBouton(event)
      let jsonDrapeauVert = {
        'mode': 'DRAPEAU_VERT'
      }
      this.websocket.send(JSON.stringify(jsonDrapeauVert))
    },
    drapeau_rouge (event) {
      this.activeBouton(event)
      let jsonDrapeauRouge = {
        'mode': 'DRAPEAU_ROUGE'
      }
      this.websocket.send(JSON.stringify(jsonDrapeauRouge))
    },
    drapeau_jaune () {
      this.activeBouton(event)
      if (this.limite > 1) this.limite = 1
      if (this.limite < 0) this.limite = 0
      let jsonDrapeauJaune = {
        'mode': 'DRAPEAU_JAUNE',
        'limite': this.limite
      }
      this.websocket.send(JSON.stringify(jsonDrapeauJaune))
    },
    drapeau_noir () {
      this.activeBouton(event)
      let jsonDrapeauNoir = {
        'mode': 'DRAPEAU_NOIR'
      }
      this.websocket.send(JSON.stringify(jsonDrapeauNoir))
    },
    drapeau_noir_damier () {
      if (this.constante > 1) this.constante = 1
      if (this.constante < 0) this.constante = 0
      this.activeBouton(event)
      let jsonDrapeauNoirDamier = {
        'mode': 'DRAPEAU_NOIR_A_DAMIER',
        'constante': this.constante
      }
      this.websocket.send(JSON.stringify(jsonDrapeauNoirDamier))
    },
    // Permet d'ajouter un style au bouton sélectionné
    activeBouton (event) {
      let boutonCible = event.srcElement
      // Si l'utilisateur a cliqué sur l'image du drapeau on récupère le parent, soit le bouton
      if (event.srcElement.localName === 'img') boutonCible = event.target.offsetParent
      // On donne une apparence active au bouton
      let boutons = document.getElementsByClassName('bouton-drapeau')
      // On réinitialise les apparences
      for (let bouton of boutons) {
        bouton.classList.remove('is-info')
      }
      // On ajoute un apparence au bouton séléctionnné si on est connecté
      if (this.state === true) boutonCible.classList.add('is-info')
    },
    // Permet de désactiver un bouton
    disabledBouton () {
      if (this.state === true) return 'disabled'
      return false
    },
    // Cette fonction permet de changer l'icône permettant d'indiquer si on accepte les connexions des voitures ou non
    isconnecte () {
      let nom = 'offline'
      // Si on est connecté
      if (this.state === true) nom = 'online'
      return this.getImgUrl(nom)
    },
    // Cette fonction permet de récupérer une image après la compilation du template
    // Elle utilisé pour afficher l'image des voitures, car on affiche leurs images après la compilation du template.
    // Après la compilation, on ne peut plus accèder aux images via le dossier assets
    // On récupère un lien du style /static/img/xxx.png
    getImgUrl (nom) {
      var images = require.context('../assets/', false, /\.png$/)
      return images('./' + nom + '.png')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
.section {
    background-color: #2c2f34;
    height: 50%;
}

.connexion-container {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    margin-bottom: 3%;
    padding-right: 50px;
    width: 400px;
}

.control-container {
    text-align: center;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
}

input {
    width: 175px;
    margin-top: 5px;
}
</style>
