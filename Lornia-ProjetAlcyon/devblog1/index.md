# Lornia : Projet Alcyon - Devblog 1

[Home](https://evury.github.io/lornia)
 | [Lornia - Projet Alcyon](https://evury.github.io/lornia/Lornia-ProjetAlcyon)
 | [Book](https://evury.github.io/lornia/Book)
 | [Lornia - MC](https://evury.github.io/lornia/Lornia-MC)
 | [Hytale](https://evury.github.io/lornia/Hytale)
 | [CodinGame](https://www.codingame.com/profile/b6e09c38b3e3ffd760cd0d21a064cfb87922051)


## **============ Devblog #1 ============**

*11/12/2021*

Premier devblog du jeu ! :D


Bon, pour le premier il n'y aura peu voir pas d'image à montrer car c'est surtout des choix techniques qu'il a fallu faire.

Mon objectif pour la première étape était de créer un écran de titre avec la possibilité d'entrer des informations afin de se connecter au serveur.


![screenshot](https://evury.github.io/lornia/Lornia-ProjetAlcyon/devblog1/img/ecrantitre.jpg)

<br/>
### **Choix du langage**

C'est très simple, j'ai choisi d'utiliser Java car c'est mon langage de prédilection et que je fais ce projet from scratch.

<br/>
### **Création du serveur**

Je commence par la couche la plus basse (la plus éloignée de l'utilisateur) qui est le serveur. Je n'utilise pas de serveur pré-fait afin de garder un maximum de contrôle dessus. Le serveur consiste à attendre et échanger des messages avec les clients. Le serveur est actuellement capable d'attendre les connexions client, de donner l'état du serveur (en ligne, en maintenance et hors ligne le cas échéant), de donner le nombre maximum de joueur pouvant se connecter et le nombre de joueur connecté.

Associé au serveur il y a la base de données qui ne contient qu'une seule table actuellement. La table *compte* qui contient les informations d'un compte.


<br/>
### **Choix des librairies**

Pour la création du jeu il me fallait un minimum de librairies. Je veux utiliser le moins d'outil possible pour devoir coder moi même le jeu et me challenger donc je n'utiliserai pas Unity, Unreal engine ou autre.

Dans le choix des libs je voulais aller à l'essentiel : affichage, jouer un son, gestion des inputs.

J'utilisais GTGE dans le passé mais il commence à être trop vieux pour les nouvelles versions de Java. J'ai donc cherché si il n'y en avait pas d'autres et je suis tombé sur *LitiEngine*. Cette lib permet de faire les choses simples que je demandais et ne propose pas de choses poussées.

Par contre gros problème avec LitiEngine, la structure est différente de ce que j'ai l'habitude d'utiliser pour créer un jeu, elle est très très récente, la documentation n'est pas complète et il y a peu d'exemple. Ça ne m'a pas empêché d'insister et de surmonter les obstacles liés à l'utilisation.


<br/>
### **Création du client**

La création du client s'est fait en plusieurs étapes afin d'avoir un écran titre.
- Background
- Menu
- Boutons
- Champs de texte
- Animations
- Message

Comme je n'ai rien pour créer rapidement j'ai dû recréer des choses simples comme un bouton cliquable.

J'ai passé beaucoup (trop) de temps sur chacun des éléments mais je pourrai facilement les réutiliser pour la suite du développement.

Le fonctionnement principal de l'écran titre est de pouvoir afficher l'état actuel du serveur, d'entrer ses identifiants pour se connecter au serveur, afficher les messages d'information comme "connexion en cours", "serveur hors ligne", etc, entrer des informations pour créer un compte (le compte créé doit être validé manuellement pour pouvoir se connecter).



Faire cet écran m'a demandé de faire quelques assets. J'ai fait vite fait les boutons et champs de texte en faisant un rectangle de couleur avec une version "survolé". Idem pour le menu et la boîte de dialogue qui afficha les messages.

La police est récupérée au pif sur le net, elle me plaisait bien.

J'ai découvert Aseprite qui permet de faire des assets 2D en pixelart, je me suis fait la main dessus en faisant une petite animation de chargement lors de la communication avec le serveur.


<br/>
### **Documentation**

Une fois que j'avais cette fenêtre complètement fonctionnelle je suis passé en mode nettoyage de code et documentation. C'est la partie longue et peu intéressante qui consiste à rendre mon code propre et lisible, d'optimiser au maximum, générer la JavaDoc.


<br/>
### **Ajouts**


A ce moment du développement ma première étape était terminée mais j'ai décidé d'ajouter quelques assets pour habiller l'écran et surtout tester. J'ai donc ajouté une musique qui se lance en boucle à cet écran et une animation de "lights" qui apparaissent en bas de l'écran et remonte jusqu'en haut en boucle, ça m'a donné une approche pour gérer les ombres en jeu.


<br/>
### **Conclusion**

En conclusion, pour le peu qui est réalisé j'ai passé beaucoup beaucoup de temps mais le temps passé dessus est réutilisable pour la suite donc je ne m'inquiète pas. Et surtout je vais pouvoir passer à des choses plus intéressantes comme la création de la physique du jeu et l'affichage de la map.

J'ai appris pas mal de chose dont je n'ai pas parlé plus haut comme la génération de log, la gestion d'un debugger et ajouter de la transparence à une image.

Voici ma fonction pour ajouter de la transparence en Java si jamais ça intéresse quelqu'un car je n'ai pas trouvé la réponse directement en cherchant sur le net :
```java
/**
 * Fonction permettant d'ajouter de la transparence à une image.
 * @param image L'image source.
 * @param alpha La valeur alpha (transparence), entre 0 et 255 (255 étant visible et 0 invisible).
 * @return l'image avec la transparence appliquée
 */
public static BufferedImage setAlpha(BufferedImage image, int alpha){ // Ma fonction est static car dans une classe d'utilitaire.

	// Parcours classique pixel par pixel de l'image.
	for (int x=0; x<image.getWidth(); x++){ 
        for (int y=0; y<image.getHeight(); y++){
		
            int r = (image.getRGB(x,y)>>16)&0xFF; // Les valeurs obtenues doivent être modifiées pour correspondre à ce qu'attend Color à sa construction
            int gr = (image.getRGB(x,y)>>8)&0xFF; // C'est un changement au niveau des bits pour éviter les nombres négatifs et obtenir une valeur entre 0 et 255.
            int b = (image.getRGB(x,y))&0xFF;
            int a = (image.getRGB(x,y)>>24)&0xFF;
			
			// Je vérifie si le pixel est déjà transparent, en testant j'ai vu qu'ajouter de la transparence à un alpha 0 donne un pixel visible.
            if(a != 0){ 
				// Je crée une couleur en prenant la couleur d'origine et en y ajoutant la nouvelle transparence.
				java.awt.Color transparent = new java.awt.Color(r ,gr ,b, alpha); 
            }else{
				// Je garde la couleur et la transparence d'origine.
            	java.awt.Color transparent = new java.awt.Color(r ,gr ,b, a); 
            }
			
			// J'applique la nouvelle couleur au pixel.
            image.setRGB(x,y, (transparent.getRGB())); 
        }
	}
    return image;
}
```
