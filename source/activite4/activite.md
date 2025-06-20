# Projet calcul de Moyenne

## Activité 4 - pouvoir ajouter des notes et des matières 

Pour rendre le programme vraiment utilisable, il faut pouvoir ajouter/supprimer des notes aux matières, et gérer les matières (création, suppression, renommage).

Pour cela nous allons utiliser des fonctions Javascript qui génèrent du code HTML.

Récupère le fichier "projetMoyenne-4.1.html"

1) Pour la matière Mathématiques uniquement, ajouter le bouton d'ajout d'une note dans le HTML

Juste avant la balise de fin de la zone de gestion des notes d'une matière :

    <div class="form-group" id="gestion-des-notes-de-XX">
        ...
        <!-- Bouton ajouter une note -->
        RAJOUTER ICI
    </div> 

(où XX correspond au code de la matière), rajouter le code HTML du bouton ci-dessous. Sur le clic du bouton, une fonction JS devra être appelée, avec en paramètre le code et le libellé de la matière que tu écriras en dur dans
un premier temps :

    <div class="row">
        <button type="button" class="btn btn-primary btn-save btn-block" style="width:50%" onclick="ajouterUneNote('0', 'Mathématiques')">➕ Ajouter une note</button>
    </div>

2) Pour les notes de Mathématiques uniquement, ajouter les boutons "supprimer" d'une note dans le HTML

Pour cela ajouter ce code HTML en dessous des balises de saisie, sous l'indication : <!--  Bouton supprimer la note -->

Le bouton déclenche une fonction javascript qui prend en paramètre le nom de la zone de note qui est composée des numéro de la matière et de la note.

    <div class="col">
        <button type="button" class="btn btn-del" onclick="supprimerNote('zone-note-0-de-0')">❌ Supprimer</button>
    </div>

3) Il faut maintenant implémenter les fonction javascript. Commençons par supprimer :

Elle prend en paramètre le nom de la zone à supprimer et supprime radicalement la balise !

Copie ce code dans la zone "script" du fichier HTML :

    function supprimerNote(nom) {
        var divZone = document.getElementById(nom);
        divZone.remove();
    }

4) Il faut maintenant implémenter la fonction d'ajout de note. Au lieu de supprimer du code
HTML, elle doit en générer, puisqu'il s'agit de créer de nouvelles balises de saisie et le bouton de suppression.

Ainsi la fonction ressemblera à :

    // code = code de la matière pour retrouver la balise où mettre les notes
    //        et pour bien identifier les zones de saisie de note pour le calcul de la moyenne
    // libelle = libellé de la matière pour le place holder de la zone de saisie
    function ajouterUneNote(code, libelle) {
            -> trouver le numéro de la nouvelle zone de note de la matière
            -> définir l'identifiant des balises 
            -> définir le code HTML des zones de saisie et du bouton
            -> l'ajouter dans la balise de la matière 
    }

    Pour trouver le numéro de la nouvelle zone de note, nous allons utiliser une variable
    globale, qui connaitra le nombre de note de chaque matière, à mettre sous la balise <script>

    let projetMoyenne = { "notesParMatieres":	[] };

    Par défaut chaque matière aura un numéro maximal de notes à 0.

    if (typeof projetMoyenne.notesParMatieres[code] == 'undefined') {
        projetMoyenne.notesParMatieres[code] = { numeroMaximal: 0 };
    }

    Ainsi nous pourrons calculer le prochain indice de la zone de note que nous incrémentons :

    const prochainId = ++projetMoyenne.notesParMatieres[code].numeroMaximal;
    
    Le nom de la zone qui contient les notes est :
    
    const idZone = 'saisie-des-notes-de-' + code;

    Nous récupérons sa balise DIV (nous faisons dans le code ce que tu as fait à la main
    le spremiers jours)

    const divZone = document.getElementById(idZone);

    Nous allons maintenant générer le code HTML qui va contenir les zones de saisie :

        const codeHtml =
        `<div class="row row-data" id="zone-note-${prochainId}-de-${code}">
            <div class="col">
              <input type="number" class="form-control"
                     id="note-${prochainId}-de-${code}" placeholder="Entrez la note de ${libelle}"
                     required value="${valeurNote}">
            </div>
            <div class="col">
              <input type="number" class="form-control" id="coeff-${prochainId}-de-${code}"
                     placeholder="Entrez le coefficient de ${libelle}" required
                     value="${valeurCoeff}">
            </div>
            <div class="col">
              <button type="button" class="btn btn-del" onclick="supprimerNote('zone-note-${prochainId}-de-${code}')">❌ Supprimer</button>
            </div>
          </div>`;

    Puis nous ajoutons ce code HTML dans la balise DIV :

    divZone.insertAdjacentHTML("beforeend", codeHtml);
    

5) Il faut maintenant mettre à jour la fonction de calcul de la moyenne, puisqu'il peut y avoir un nombre indéterminé de notes.

Nous allons devoir définir le nombre de matières maximum dans la variable globale en rajoutant :

    "numeroMaximal": 0

Puis dans la fonction de calcul de moyenne par matière, nous allons chercher le nombre maximum de notes dans la variable globale :

    const valeurMaximale = projetMoyenne.notesParMatieres[codeMatiere].numeroMaximal;

Par défaut il y a 2 notes, donc il faut initialiser le numéro maximal :

    projetMoyenne.notesParMatieres[0].numeroMaximal = 1;
    projetMoyenne.notesParMatieres[1].numeroMaximal = 1;
    projetMoyenne.notesParMatieres[2].numeroMaximal = 1;
    projetMoyenne.notesParMatieres[3].numeroMaximal = 1;

6) Une amélioration possible pour re-calculer la moyenne à chaque interaction utilisateur est d'appeler le calcul sur l'évènement "onChange" :

    onChange="calculerMoyenneGenerale(3)"


Solution dans le fichier "projetMoyenne-4.2.html"