<template>
<section class="section display-section">
    <div class="container display-container">
        <div id="circuit">
            <img id="image-circuit" class="image" src="../assets/circuit.png">
            <img
                v-for="voiture in voitures"
                v-bind:key="voiture.id"
                :id="voiture.id"
                class="image voiture" :src="getImgUrl(voiture.id)"
            />
        </div>
    </div>
    <div class=" container display-container">
        <div 
            v-for="voiture in voitures"
            v-bind:key="voiture.id" 
            class="container-voiture"
        >
            <img class="image is-128x128" :src="getImgUrl(voiture.id)">
            <p class="title is-5 voiture-info">Voiture : {{voiture.id}}</p>
            <p class="subtitle is-small voiture-info"> Position: {{voiture.position}}</p>
        </div>
    </div>
</section>
</template>

<script>
export default {
    name: 'DisplayComponent',
    data() {
        return {
            position: 0,
            voitures: []
        }
    },
    props: {
      voiture: {
        type: Object
      },
      state: {
        type: Boolean
      }
    },
    watch: {
      // Watch permet de surveiller des variables
      // quand une varaible change de valeur on lance une fonction
      // ici quand l'état de la voiture change on effectue cette fonction: 
      voiture: function (v) {
        // On regarde le mode
        switch (v.mode) {
            // Quand la voiture a été déco
            case 'DECONNEXION':
                this.supprimerVoiture(v.id)
            break;
            // Quand on recoit une position
            default:
                // On regarde si c'est la première connexion de la voiture
                // et on ajoute la voiture sur le circuit
                this.ajoutVoiture(v)
                // On ajoute la position dans les données du composant
                this.updateVoiture(v)
                // On change la position de la voiture sur le circuit
                this.mouvementVoiture(v.position, v.id)
            break;
        }
      },
      state: function(state) {
          // Si on s'est déconnecté
          if(state === false){
              this.voitures = [];
          }
      }
    },
    methods: {
        // Faire avancer la voiture à la position donnée
        mouvementVoiture(position, id) {
            // TODO: vérifier si position compris entre 0 et 1

            // Calcul de l'ordonnée et de l'abscisse
            // on inverse pour aller dans le sens positif
            position = 1 - position
            // Récupération des coordonnées
            let x = Math.cos(2 * Math.PI * position) * 100
            let y = Math.sin(2 * Math.PI * position) * 100

            let voitureImg = document.getElementById(id)

            // On place la voiture sur le circuit
            voitureImg.style.left = 120 + x + "px";
            voitureImg.style.top = 120 + y + "px";
        },
        ajoutVoiture(voiture){
            // On regarde si la voiture est déjà ajoutée
            if(this.voitures.length === 0){
                this.voitures.push(voiture)
            }
            else{
                let existe = false;
                this.voitures.forEach(v => {
                    if(v.id === voiture.id) existe = true
                });
                if(!existe) this.voitures.push(voiture)
            }   
        },
        updateVoiture(voiture){
            this.voitures.forEach((v, index) => {
                if(voiture.id === v.id){
                    this.voitures[index].position = parseFloat(voiture.position.toFixed(2))
                }
            });
        },
        supprimerVoiture(id) {
            this.voitures.forEach((voiture, index) => {
                if(voiture.id === id){
                    // supprimer la voiture
                    this.voitures.splice(index,1)
                }
            });
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
    text-align: center;
}

.display-container {
    border: #1290dd solid 2px;
    border-radius: 10px;
    width: 600px;
    height: 300px;
    display: flex;
    flex-direction: column;
    overflow: auto;
    margin-left: 10px;
}

.container-voiture{
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

#circuit{
    position: relative;
    margin: auto;
    width: 250px;
    height: 250px;
}

#image-circuit{
    height: 250px;
    width: 250px;
}

.voiture{
    position: absolute;
    left: 220px;
    top: 125px;
    height: 20px;
    width: 30px;
}

.voiture-info{
    color: #1290dd;
}
</style>
