# Projet calcul de Moyenne

## Activité 3 - modifier la fonction javascript

Rajouter des notes et des matières est fastidieux, il faut l'automatiser.

Nous allons commencer à modifier la fonction Javascript du calcul de la moyenne et faire en sorte qu'elle puisse gérer un nombre de notes et de matières non limité. 

Pour cela nous allons utiliser des boucles en Javascript.

1) Avant de modifier le programme, forme toi un peu sur un site de référence :

https://www.w3schools.com/js/

2) Reprends le fichier de la veille, et créé une fonction Javascript "calculerMoyenneDuneMatiere" qui calcule la moyenne d'une matière de n notes.

Elle prend en paramètre le code d'une matière (0, 1, ...) et va chercher tous les champs possibles de 0 à une valeur maximale (définie dans une variable), puis retourne la moyenne.

Voici le code expliqué que tu doit écrire dans la zone "Javascript" du fichier :

    function calculerMoyenneDuneMatiere(codeMatiere) -- elle prend en paramètre le numéro de la matière 
        initialier la variable qui contient le résultat "moyenne". Elle vaut null par défaut car s'il n'y a aucune note, la fonction retourne "null"
            let moyenne = null;

        initialiser la variable "valeurMaximale" qui contient le nombre de note maximum
            const valeurMaximale = 2;
        
        initialiser les variables qui vont contenir la somme des notes et la somme des coefficients
            let sommeNotes = 0;
            let sommeCoeff = 0;

        Pour toutes les valeurs i entre 0 et valeurMaximale (utiliser une boucle for)
            for (var i = 0; i <= valeurMaximale; i++) {
    
            chercher la valeur de la zone de saisie d'identifiant : 'note-' + i + '-de-' + codeMatiere
                let inputNote = document.getElementById('note-' + i + '-de-' + codeMatiere);
            chercher la valeur de la zone de saisie d'identifiant : 'coeff-' + i + '-de-' + codeMatiere
                let inputCoeff = document.getElementById('coeff-' + i + '-de-' + codeMatiere);
    
            si zones existent et ces valeurs sont numériques, les additionner 
                if (inputNote != null && inputCoeff != null) {
                    let note = inputNote.value;
                    let coeff = inputCoeff.value;
                    sommeNotes = sommeNotes + note * coeff;
                    sommeCoeff = sommeCoeff + coeff * 1;
                }
            
            Fermer la boucle for par une accolade fermante "}"
        
        Si on a trouvé des coefficients on peut calculer la moyenne
            if (sommeCoeff > 0) {
                moyenne = sommeNotes / sommeCoeff;
            }
    
        Retourner la variable moyenne
            return moyenne;


3) Modifie la fonction de calcul de la moyenne générale "calculerMoyenneGenerale" pour qu'elle appelle le calcule de la moyenne d'une matière pour chaque
matière, de 0 à une valeur maximale (définie dans une variable).

Voici le "pseudo code', à toi d'écrire le bon code Javascript dans le fichier :

    function calculerMoyenneGenerale(valeurMaximaleMatiere) 
        prend en paramiètre valeurMaximaleMatiere qui est l'indice de la dernière matière 

        Initialiser les variables qui seront utilisées :
            sommeDesMoyennes à 0 par défaut, contiendra la somme des moyennes trouvée pour chaque matière
            nombreDeMatiere à 0 par défaut, contiendra le nombre de matière trouvées pour le calcul de la moyenne
            
        Pour toutes les matières i de 0 à "valeurMaximaleMatiere" :
            récupérer la moyenne de la matère 
            aditionner la moyenne de chaque matière dans la variable sommeDesMoyennes pour la diviser à la fin par le nombre de matière
            augmenter la valeur de nombreDeMatiere 

        Si le nombre de matière "nombreDeMatiere" est supérieur à 0, calculer la moyenne générale : sommeDesMoyennes / nombreDeMatiere;
        L'affecter dans la zone HTML

4) Donner en paramètre d'appel à la fonction "calculerMoyenneGenerale" dans le "onclick" du bouton de calcul la valeur 3 car nous avons des matières de 0 à 3 et elle attend en paramètre l'indice le plus élevé :

    onclick="calculerMoyenneGenerale(3)"

Solution dans le fichier "projetMoyenne-3.1.html"