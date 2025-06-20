# Projet calcul de Moyenne

## Activité 2 - étudier la première version du projet

1) Ouvrir le fichier "projetMoyenne-1.1.html" dans un editeur de texte ou un environnement de développement
2) Ouvrir le fichier dans un navigateur
3) Quelles sont les interactions possibles avec l'utilisateur ?
4) Quelles sont les améliorations qui seraient nécessaires ?
5) Peux-tu expliquer la structure de la page HTML, par exemple avec un schéma sur une feuille ?
Exemple :

        - Style                                              
        - Body                                               
            - Titre H2                                          
            - Zone de matière id=gestion-des-notes-de-0       
                - Titre de matière id=matiere-0                   
                - Zone de noteS de matière id=saisie-des-notes-de-0
                  - Zone de 1ère note id=zone-note-0-de-0          
                  - Zone de 2ème note id=zone-note-1-de-0          
        ...

6) Quelle est le nom de la fonction Javascript qui est appelée sur l'évènement "onClick" du bouton de calcul de la moyenne ?
7) Peux-tu expliquer comment se réalise le calcul de la moyenne ?
8) Peux-tu rajouter les 2 notes d'histoire dans le calcul de la moyenne ?

Indice : il faut dupliquer une zone HTML autour des la ligne 167 à 190 et de la copier à la ligne 206, et modifier les valeurs des attrbuts "id" des balises HTML pour indiquer qu'il s'agit de la matière de code "3"
Changer le texte "français" en "histoire"
Ex : "note-0-de-2" devient "note-0-de-3"
Puis il faut modifier la fonction JavaScript de calcul de la moyenne

Solution dans le fichier "projetMoyenne-2.1.html"