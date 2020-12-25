<template>
<section class="section display-section">
    <div class="container display-container">
        <div id="circuit">
            <img id="image-circuit" class="image" src="../assets/circuit.png">
            <img id="1" class="image voiture" src="../assets/tesla.png">
            <img id="2" class="image voiture" src="../assets/tesla.png">
        </div>
    </div>
    <div class=" container display-container">
        {{voitures}}
        <div 
            v-for="voiture in voitures"
            v-bind:key="voiture.id" 
            class="container-voiture"
        >
            <img class="image is-128x128" src="../assets/tesla.png">
            <p class="title is-5 voiture-info">Voiture {{voiture.id}}: Tesla</p>
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
      }
    },
    watch: {
      // Watch permet de surveiller des variables
      // quand une varaible change de valeur on lance une fonction
      // ici quand l'état de la voiture change on effectue cette fonction: 
      voiture: function (v) {
        // On regarde si c'est la première connexion de la voiture
        // et on ajoute la voiture sur le circuit
        this.ajoutVoiture(v)
        // On ajoute la position dans les données du composant
        this.updateVoiture(v)
        //this. = parseFloat(v.position.toFixed(2))
        // On change la position de la voiture sur le circuit
        // TODO: gérer plusieurs voitures
        this.mouvementVoiture(v.position, v.id)
      }
    },
    methods: {
        // Faire avancer la voiture à la position donnée
        mouvementVoiture(position, id) {
            // TODO: vérifier si position compris entre 0 et 1

            // TODO: Récupération de la voiture en fonction de l'id

            // Calcul de l'ordonnée et de l'abscisse
            // on inverse pour aller dans le sens positif
            position = 1 - position
            // Récupération des coordonnées
            let x = Math.cos(2 * Math.PI * position) * 100
            let y = Math.sin(2 * Math.PI * position) * 100

            // TODO: créer la voiture dans le code Js
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
                    this.voitures[index].position = voiture.position
                }
            });
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
