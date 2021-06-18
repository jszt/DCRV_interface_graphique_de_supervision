# DCRV_interface_graphique_de_supervision
Interface de supervision permettant de contrôler les voitures via le serveur 


Le “superviseur global” est composé d’une IHM appelée l’interface graphique de supervision et d’un serveur Web permettant de gérer la logique de l’IHM, comme la connexion/déconnexion des voitures, le calcul de la position, … 

L'interface graphique de supervision, est utile pour afficher les données de nos voitures, comme son état(connectée, connectée mais pas en course, en course), sa vitesse réelle(la vitesse du moteur de la voiture), sa position sur le circuit et le nombre de tours que la voiture a effectuée.

![image](https://user-images.githubusercontent.com/33723014/122560724-3c6a8e80-d041-11eb-8496-8570921bd474.png)


Sur l’interface graphique de supervision, on peut observer plusieurs parties.

Premièrement, il y a le circuit.

Le circuit permet simplement de visualiser la position des voitures.

Ensuite, il y a la partie de contrôle, avec plusieurs boutons permettant de choisir le mode de course.

Dans cette partie il y a deux boutons: “Se connecter” et “Se déconnecter”. Ces boutons permettent de connecter l’interface de supervision au serveur Web et ainsi d'écouter les connexions entrantes de voitures.

Quand l’icône représente une connexion non établie, cela signifie que l’interface n’est pas connecté au serveur.

Quand l’icône représente une connexion établie, cela signifie que la connexion avec le serveur a été effectuée.

Concernant les modes de courses:

Voici leurs fonctions:
 
- Mode drapeau vert : Donne la main à l’utilisateur pour contrôler la voiture avec son application.
- Mode drapeau jaune : L’utilisateur garde le contrôle mais le superviseur global limite la vitesse des voitures avec une vitesse choisie par l’arbitre.
- Mode drapeau rouge : Le superviseur global prend le contrôle de la flotte de voitures et envoie une consigne vitesse de 0 afin d’arrêter toutes les voitures de la flotte. 
- Mode drapeau noir : Le superviseur global prend le contrôle et ramène toutes les voitures sur la ligne de départ . 
- Mode drapeau à damier : Le superviseur global prend le contrôle et fait entrer les voitures en mode démo. Ces dernières se suivent  à une vitesse constante choisie par l’arbitre. 
- La dernière partie concerne les informations sur les voitures. Si une voiture n’est pas connectée elle apparait grisée, si elle est connectée, elle apparait non grisée.

Quand une voiture est connectée, il est possible de la faire rentrer dans la course. Il faut appuyer sur le bouton “course”. Une fois en course la voiture apparait sur le circuit et on peut la commander.

Pour déconnecter une voiture il faut appuyer sur le bouton Déconnexion. La voiture peut prendre du temps à se déconnecter(dans l’ordre de 5 à 10 secondes en fonction de la carte de la voiture), pour accélérer ce temps, il faut passer en mode drapeau rouge pour arrêter la voiture ou la sortir de la course, mais ce n’est pas obligatoire.
