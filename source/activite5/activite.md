
1) Maintenant il ne reste plus qu'à rentre dynamique l'ajout ou suppression des matières !

    Pour cela il faut rajouter le bouton d'action pour ajouter une matière :

        <button type="button" class="btn btn-primary btn-save btn-block" style="width:100%" onclick="ajouterMatierePourUtilisateur()">➕ Ajouter une matière</button>

    Pour connaître le nombre de matière présentes à l'écran, nous allons compléter notre variable globale qui contiendra l'attribut "numeroMaximal" à la racine, correspondant au nombre de matières, ce qui donne :  

        let projetMoyenne = { 
            "numeroMaximal": 0,
            "notesParMatieres":	[]
        };

        /* exemple de valorisation :
        *	{ numeroMaximal:1,
        *    notesParMatieres: [ { libelle:"Mathématiques", numeroMaximal:2, notes: [{ note:15, coeff:1 }, 
        *                                                                            { note:12, coeff:3 }] },
        *                        { libelle:"Français", numeroMaximal:0 } ]
        *  }
        */    

2) Puis il faut définir la méthode qui sera appelée : 

       /**
        * Fonction permettant de rajouter le code HTML d'une matière
        * Une matiere est composée d'un titre, des boutons de modification du titreet de suppression et du bouton d'ajout de note
        */
        function ajouterMatiere(libelleMatiere)
   
    Elle va incrémenter le compteur de matières 
    
        const code = projetMoyenne.numeroMaximal;
        projetMoyenne.numeroMaximal = code + 1;

    
    Et fabriquer le code HTML :

        const codeHtml =
            `<div class="form-group" id="gestion-des-notes-de-${code}">
             <div class="row">
                    <h5 id="matiere-${code}">${libelleMatiere}</h5>
                    <button class="edit-button" aria-label="Modifier" onClick="modifierNomMatierePourUtilisateur('${code}')">
                        <span class="icon">✏</span>
                    </button>
                    <button class="edit-button" aria-label="Supprimer" onClick="supprimerMatierePourUtilisateur('${code}')">
                        <span class="icon">️❌</span>
                    </button>

                </div>
                <div class="row">
                    <div class="col">
                        <label>Note</label>
                    </div>
                    <div class="col">
                        <label>Coefficient</label>
                    </div>
                </div>
                <div id="saisie-des-notes-de-${code}">
                </div>
                <div class="row">
                    <button type="button" class="btn btn-primary btn-save btn-block" style="width:50%" onclick="ajouterUneNote('${code}', '${libelleMatiere}', '', '')">➕ Ajouter une note</button>
                </div>
            </div>`;
        
    Ce code HTML est inséré dans le formulaire par l'instruction suivante :
    
        document.getElementById('formulaire-des-notes').insertAdjacentHTML("beforebegin", codeHtml);

3) Avant d'ajouter une matière il faut capter son libellé ! Rajoute cette autre fonction :

        /**
        * Interagit avec l'utilisateur pour capter le libellé d'une matière à ajouter
        * et l'ajoute dans le HTML
        */
        function ajouterMatierePourUtilisateur() {
            var nouvelleMatiere = window.prompt("Nom de la nouvelle matière ?");
            if (nouvelleMatiere != "") {
                ajouterMatiere(nouvelleMatiere);
            }
        }

4) Implémenter les fonctions  pour supprimer une matière à partir du code de la matière. 
Il faut modifier également la variable pour intégrer la suppression de cette matière.

       /**
       * Interagit avec l'utilisateur pour valider la demande de suppression d'une matière
       * et l'enlève du HTML
       */
       function supprimerMatierePourUtilisateur(code) {
            var divMatiere = document.getElementById(`gestion-des-notes-de-${code}`);
            divMatiere.remove();
            //Si une matière a été supprimée, elle reste dans le tableau à null, ce qui permet de ne pas impacter les indices du tableau
            //il faut donc l'ignorer
            projetMoyenne.notesParMatieres[code] = null;
      }

5) Mettre à jour la fonction de calcul de moyenne générale : il ne faut plus passer en paramètre le nombre de matières, mais le récupérer depuis la variable globale :

        for (let codeMatiere in projetMoyenne.notesParMatieres) {
            var moyenneDeLaMatiere = calculerMoyenneDuneMatiere(codeMatiere);
            if (moyenneDeLaMatiere != null) {
                sommeDesMoyennes = sommeDesMoyennes + moyenneDeLaMatiere;
                nombreDeMatiere = nombreDeMatiere + 1;
            }
        }

6) Pour ne pas perdre les données saisies à chaque rechargement de la page, il faut les stocker dans la mémoire du navigateur, associé à une clé :

        // Clé utilisée pour associer la variable "projetMoyenne" dans la mémoire du navigateur
        const CLE_DE_STOCKAGE = "projetMoyenne";

Et ajouter une fonction de sauvegarde qui doit être appelée à chaque interaction (saisie, ajout de note, de matière) :

    /**
    * Récupère les notes du HTML pour les mettre dans l'objet "projetMoyenne"
    * puis stocke cet objet dans la mémoire du navigateur
    */
    function sauvegarder() {
        for (let codeMatiere in projetMoyenne.notesParMatieres) {
            //Si une matière a été supprimée, elle reste dans le tableau à null, ce qui permet de ne pas impacter les indices du tableau
            //il faut donc l'ignorer
            if (projetMoyenne.notesParMatieres[codeMatiere] != null) {
                let tableauDeNotes = [];
                const valeurMaximale = projetMoyenne.notesParMatieres[codeMatiere].numeroMaximal;
                projetMoyenne.notesParMatieres[codeMatiere].notes = tableauDeNotes;

                for (var i = 0; i <= valeurMaximale; i++) {
                    let inputNote = document.getElementById('note-' + i + '-de-' + codeMatiere);
                    let inputCoeff = document.getElementById('coeff-' + i + '-de-' + codeMatiere);
                    if (inputNote != null && inputCoeff != null) {
                        let note = inputNote.value;
                        let coeff = inputCoeff.value;
                        tableauDeNotes.push({ "note":note, "coeff":coeff });
                    }
                }
            }
        }

        localStorage.setItem(CLE_DE_STOCKAGE, JSON.stringify(projetMoyenne));
    }


Et une fonction de chargement :

    /**
    * Récupère les données depuis l'objet passé en paramètre et les installe dans le HTML
    * en appelant les fonctions d'ajout d'une matière et d'ajout des notes
    * param : l'objet JSON récupéré de la mémoire
    */
    function chargerDuStockage(projetMoyenneStockage) {
        var notesParMatieres = projetMoyenneStockage.notesParMatieres;
        projetMoyenne = projetMoyenneStockage;
        projetMoyenne.notesParMatieres = [];
        projetMoyenne.numeroMaximal = 0;
        for (let indiceMatiere in notesParMatieres) {
            let matiere = notesParMatieres[indiceMatiere];
            if (matiere != null) {
                let codeMatiere = ajouterMatiere(matiere.libelle);

                for (let indiceNotes in matiere.notes) {
                    ajouterUneNote(codeMatiere, matiere.libelle, matiere.notes[indiceNotes].note, matiere.notes[indiceNotes].coeff);
                }
            }
        }
        calculerMoyenneGenerale();
    }

La fonction de chargement doit être appelée à l'initialisation de la page.
Il faut mettre dans la balise "body" l'appel suivant : onLoad="initialiserLaPage()"
Puis définir l'appel :

    /**
    * Fonction appelée lorsque la page est chargée
    */
    function initialiserLaPage() {
        let projetMoyenneStorage = localStorage.getItem(CLE_DE_STOCKAGE);
        if (projetMoyenneStorage != null) {
            chargerDuStockage(JSON.parse(projetMoyenneStorage));
        } else {
            const matieresParDefaut = [ "Mathématiques", "Physique",  "Français", "Histoire" ];
            matieresParDefaut.forEach(matiere => ajouterMatiere(matiere));
        }
    }



Correction dans le fichier [projetMoyenne-5.1.html](projetMoyenne-5.1.html)