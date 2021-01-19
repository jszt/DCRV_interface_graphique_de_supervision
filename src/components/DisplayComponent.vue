<template>
<section class="section display-section">
    <div class="container circuit-container">
        <div id="circuit">
            <img 
                id="image-circuit" 
                class="image" 
                src="../assets/circuit.png"
            >
            <img
                v-for="voiture in voitures"
                v-bind:key="voiture.id"
                v-bind:id="voiture.id"
                class="image voiture" 
                v-bind:src="getImgUrl(voiture.id)"
                v-if="voiture.etat === 2"
            />
        </div>
    </div>
    <div class="container voitures-container">
        <div 
            v-for="voiture in voitures"
            v-bind:key="voiture.id" 
            class="container-voiture"
            v-bind:class="checkEtat(voiture)"
        >
            <img class="image image-voiture" :src="getImgUrl(voiture.id)">
            <p class="voiture-info">Voiture : {{voiture.id}}</p>
            <p class="is-small voiture-info"> Position: {{voiture.position}}</p>
            <p class="is-small voiture-info"> Vitesse: {{voiture.vitesse}}</p>
            <p class="is-small voiture-info"> Tour: {{voiture.tour}}</p>
            <button 
                class="button is-success is-rounded is-outlined bouton-voiture" 
                v-if="voiture.etat === 1"
                v-on:click="changerEtatVoiture(voiture, 2)"
            >
                Course
            </button>
            <button 
                class="button is-warning is-rounded is-outlined bouton-voiture" 
                v-if="voiture.etat === 2"
                v-on:click="changerEtatVoiture(voiture, 1)"
            >
                Stop course
            </button>
            <button 
                class="button is-danger is-rounded is-outlined bouton-voiture" 
                v-if="voiture.etat >= 1"
                v-on:click="supprimerVoiture(voiture, $event)"
            >
                Déconnexion
            </button>
        </div>
    </div>
</section>
</template>

<script>
export default {
    name: 'DisplayComponent',
    data() {
        return {
            voitures: [
                {
                    id: '208',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'audi_etron',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'bmw_i8',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'citroen_e_c4',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'cybertruck',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'ds3_crossback_e_tense',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'e_golf',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'jaguar_i_pace',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'lexus_ux300e',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'mercedes_eqc',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'mini_cooper_se',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'model_3',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'model_s',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'porsche-taycan',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'roadster',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
                {
                    id: 'zoe',
                    position: 0,
                    vitesse: 0,
                    tour: 0,
                    etat: 0,
                },
            ]
        }
    },
    props: {
      voiture: {
        type: Object
      },
      state: {
        type: Boolean
      },
      websocket: {
        type: WebSocket
      }
    },
    watch: {
      // Watch permet de surveiller des variables
      // quand une varaible change de valeur on lance une fonction
      // ici quand l'état de la voiture change on effectue cette fonction: 
      voiture: function (v) {
        // On regarde le mode
        switch (v.mode) {
            // Quand le serveur nous a spécifié la deconnexion de la voiture
            case 'DECONNEXION':
                alert('ok')
                let voiture = this.recuperationVoiture(v.id)
                voiture.vitesse = 0
                voiture.position = 0
                voiture.tour = 0
                voiture.etat = 0
            break;
            // Quand on recoit une position
            default:
                // On regarde si c'est la première connexion de la voiture
                // et on ajoute la voiture sur le circuit
                this.ajoutVoiture(v)
                // On ajoute la position dans les données du composant
                //this.updateVoiture(v)
                // On change le classement des voitures dans la course
                this.updateClassement()
                // On change la position de la voiture sur le circuit
                this.mouvementVoiture(v)
            break;
        }
      },
      state: function(state) {
          // Si on s'est déconnecté
          // On affiche toues les voitures déconnectées
          if(state === false){
              this.voitures.forEach((v, index) => {
                  this.voitures[index].etat = 0;
              });
          }
      }
    },
    methods: {
        // Faire avancer la voiture à la position donnée
        mouvementVoiture(v) {
            //TODO: return si voiture pas mode 2
            let voiture = this.recuperationVoiture(v.id)
            if(voiture.etat != 2) return

            voiture.position = parseFloat(v.position).toFixed(2)
            voiture.vitesse = parseFloat(v.vitesse).toFixed(2)
            voiture.tour = v.tour

            // Calcul de l'ordonnée et de l'abscisse
            // on inverse pour aller dans le sens positif
            let position = 1 - v.position
            // Récupération des coordonnées
            let x = Math.cos(2 * Math.PI * position) * 125
            let y = Math.sin(2 * Math.PI * position) * 125

            let voitureImg = document.getElementById(v.id)

            // On place la voiture sur le circuit
            voitureImg.style.left = 135 + x + "px";
            voitureImg.style.top = 155 + y + "px";
        },
        ajoutVoiture(voiture){
            // On regarde si la voiture est déjà ajoutée
            /*if(this.voitures.length === 0){
                this.voitures.push(voiture)
            }
            else{
                let existe = false;
                this.voitures.forEach(v => {
                    if(v.id === voiture.id) existe = true
                });
                if(!existe) this.voitures.push(voiture)
            }*/
            
            
            this.voitures.forEach((v, index) => {
                if((v.id === voiture.id) && v.etat != 2) this.voitures[index].etat = 1;
            });
            
        },
        updateVoiture(voiture){
            this.voitures.forEach((v, index) => {
                if(voiture.id === v.id){
                    this.voitures[index].position = parseFloat(voiture.position).toFixed(2)
                    this.voitures[index].vitesse = parseFloat(voiture.vitesse).toFixed(2)
                    this.voitures[index].tour = voiture.tour
                }
            });
        },
        updateClassement() {
            // On trie le tableau des voitures en fonction de l'etat de la voiture
            this.voitures.sort(function (a, b) {
                return b.etat - a.etat;
            });
            // On trie le tableau des voitures en fonction de la position et des tours
            this.voitures.sort(function (a, b) {
                if(a.tour >= b.tour) return b.position - a.position;
            });
        },
        // Cette fonction permet de chercher une voiture dans le tableau répertoriant toutes les voitures
        // grâce à un id spécifié en paramètre
        recuperationVoiture(id){
            
            if(!id) return null

            let voitureTrouvee = null
            this.voitures.forEach(v => {
                if(v.id === id){
                voitureTrouvee = v
                } 
            })
            return voitureTrouvee
        },
        supprimerVoiture(voiture, event) {
            // L'ESP32 peut être long à se fermer
            // On change la classe du bouton de connexion pour que l'untilisateur n'appuie pas deux fois
            // On envoie une requête au serveur pour déconnecter la voiture
            console.log(event.srcElement);
            event.srcElement.classList.add('is-loading')
            event.srcElement.setAttribute('disabled', 'disabled');
            this.websocket.send(JSON.stringify({
                mode: 'DECONNEXION_VOITURE',
                id: voiture.id
            }))
        },
        // Cette fonction permet de regarder l'état d'un voiture et de changer le style de celle-ci en fonction de l'état
        checkEtat(voiture){
            let classe = []
            switch (voiture.etat) {
                case 0:
                    // La voiture n'est pas connectée
                    classe.push('non-connectee')
                break;

                case 1:
                    // La voiture est connectée
                    classe.push('connectee')
                break;

                case 2:
                    // La voiture est dans la course
                    classe.push('course')
                break;
            }
            return classe
        },
        changerEtatVoiture(voiture, etat){
            // Si on enlève la voiture de la course on la réinitialise
            if((voiture.etat === 2 & etat === 1)) {
                voiture.position = 0
                voiture.vitesse = 0
                voiture.tour = 0
            }
            voiture.etat = etat
            this.websocket.send(JSON.stringify({
                mode: 'CHANGER_ETAT',
                id: voiture.id,
                etat: etat
            }))
        },
        // Cette fonction permet de récupérer une image après la compilation du template
        // Elle utilisé pour afficher l'image des voitures, car on affiche leurs images après la compilation du template.
        // Après la compilation, on ne peut plus accèder aux images via le dossier assets
        // On récupère un lien du style /static/img/xxx.png
        getImgUrl(id){
            var images = require.context('../assets/', false, /\.png$/)
            return images('./' + id + ".png")
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
.display-section {
    display: flex;
    flex-wrap: wrap;
    background-color: #2c2f34;
}

.circuit-container{
    width: 300px;
    height: 500px;
    display: flex;
    border: #1290dd solid 2px;
    border-radius: 10px;
    flex-direction: column;
    overflow: auto;
}

.voitures-container{
    width: 600px;
    height: 500px;
    overflow: auto;
    border: #1290dd solid 2px;
    border-radius: 10px;
    margin-left: 10px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-around;
    flex-wrap: wrap;
}

.container-voiture{
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 10px 5px 10px 5px;
    min-width: 150px;
}

.image-voiture{
    width: 70px;
    height: 50px;
}

#circuit{
    position: relative;
    margin: auto;
    width: 350px;
    height: 350px;
}

#image-circuit{
    height: 350px;
    width: 350px;
}

.voiture{
    position: absolute;
    left: 275px;
    top: 160px;
    height: 30px;
    width: 60px;
}

.voiture-info{
    color: #1290dd;
}

.bouton-voiture{
    width: 70px;
    font-size: 10px;
}

.non-connectee{
    opacity: 0.3;
}
.non-connectee > p{
    color: gray;
}

</style>
