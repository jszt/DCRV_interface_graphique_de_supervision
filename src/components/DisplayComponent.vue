<template>
<section class="section display-section">
    <div class="container display-container">
        <div id="circuit">
            <img id="image-circuit" class="image" src="../assets/circuit.png">
            <img id="voiture" class="image voiture" src="../assets/tesla.png">
        </div>
    </div>
    <div class=" container display-container">
        <div class="container-voiture">
            <img class="image is-128x128" src="../assets/tesla.png">
            <p class="title is-5 voiture-info">Voiture 1: Tesla</p>
            <p class="subtitle is-small voiture-info"> Position: {{position}}</p>
        </div>
    </div>
</section>
</template>

<script>
export default {
    name: 'DisplayComponent',
    data() {
        return {
            position: 0
        }
    },
    props: {
      position: {
        type: String
      }
    },
    watch: {
      // Watch permet de surveiller des variables
      // quand une varaible change de valeur on lance une fonction
      // ici quand la position change on effectue cette fonction: 
      position: function (p) {
        // On ajoute la position dans les données du composant
        this.position = p
        // On change la position de la voiture sur le circuit
        // TODO: gérer plusieurs voitures
        this.mouvementVoiture(p, null)
      }
    },
    methods: {
        // Faire avancer la voiture à la position donnée
        mouvementVoiture(position, voiture) {
            // TODO: vérifier si position compris entre 0 et 1

            // TODO: Récupération de la voiture en fonction de l'id

            // Calcul de l'ordonnée et de l'abscisse
            // on inverse pour aller dans le sens positif
            position = 1 - position
            // Récupération des coordonnées
            let x = Math.cos(2 * Math.PI * position) * 100
            let y = Math.sin(2 * Math.PI * position) * 100

            // TODO: créer la voiture dans le code Js
            voiture = document.getElementById("voiture")

            // On place la voiture sur le circuit
            voiture.style.left = 120 + x + "px";
            voiture.style.top = 120 + y + "px";
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
