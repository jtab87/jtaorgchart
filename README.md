# Jtaorgchart

# Description
This plugin allows displaying a hierarchical chart based on a data source.

<img src="./src/images/1.png" width="800px" height="400px" />

# Usage

- **Mouse wheel**: zoom in/out
- **Click + drag**: moves the chart within the window
- **Click on the card**: Triggers the "Click on card" onclick event
- **Click on "/\\" or "\\/" button**: Shows/hides the children of that card
- **Search**: A field allows entering all or part of a name (if "**With search**" = true). The corresponding cards will appear.

# Configuration

| Parameter           | Description                                              |
| :------------------ | :------------------------------------------------------- |
| **Initial Zoom**    | Initial zoom level for displaying the chart. Default is 1 |
| **With search**     | true to display the search field                          |
| **Click + select**  | true to visually identify the selected card and center it in the window |
| **Click on card**   | The "Onclick" event allows accessing the record corresponding to the card (jtaOrgchart.card) |
| **Dataprovider**    | The data source. (_Required_)                            |
| **id**              | The field corresponding to the card's ID. (_Required_)    |
| **ParentId**        | The field corresponding to the card's parent ID. (_Required_) The "root" card will be the one without a parent (empty field) |
| **Name**            | The field corresponding to the name. (_Required_)         |
| **Grade**           | The field corresponding to the grade. (_Required_)        |
| **Miscellaneous**   | The "miscellaneous" field. (_Required_)                  |
| **With image**      | true to display an image associated with the card         |
| **Image**           | The field containing the image. Must be of type **"Attachments"**. The retained image is the first one found in this field. If no image is found, it will be replaced by a generic icon |
| **Background color, name color, grade color, miscellaneous field color** | The colors to use |

<img src="./src/images/2.png" width="800px" height="400px" />

## Instructions

To build your new  plugin run the following in your Budibase CLI:
```
budi plugins --build
```

You can also re-build everytime you make a change to your plugin with the command:
```
budi plugins --watch
```

Find out more about [Budibase](https://github.com/Budibase/budibase).

<hr>

# Version française

<hr>

# Description
Ce plugin permet d'afficher un organigramme hiérarchique à partir d'une source de données.

<img src="./src/images/1.png" width="800px" height="400px" />

# Usage

- **Molette souris** : zoom +/-
- **Cliquer + tirer** : déplace l'organigramme dans la fenêtre
- **Clic sur la fiche** : Déclenche le onclick "Click sur fiche"
- **Clic sur le bouton "/\\" ou "\\/"** : Affiche / masque les fils de cette fiche
- **Recherche** : Un champ permet de saisir tout ou partie d'un nom (Si "**Avec recherche**" = true). Les fiches qui correspondent apparaissent

# Configuration

| Paramètre  | Description |
| :--------------- |:--------------- |
| **Zoom initial**  | Le zoom initial d'affichage de l'organigramme. 1 par défaut |
| **Avec recherche**  |  true pour faire apparaitre le champ de recherche |
| **Clic + sélection**  |  true pour que la fiche sélectionnée soit identifiée visuellement et se place au centre de la fenêtre |
| **Clic sur fiche**  |  L'event "Onclick" permet d'accéder à l'enregistrement correspondant à la fiche (jtaOrgchart.fiche)  |
| **Dataprovider**  |  La source de données. (_Obligatoire_) |
| **id**  |  Le champ correspondant à l'ID de la fiche. (_Obligatoire_) |
| **ParentId**  |  Le champ correspondant à l'ID du père de la fiche. (_Obligatoire_). La fiche "racine" sera celle qui n'a pas de père (champ vide) |
| **Nom**  |  Le champ correspondant au nom. (_Obligatoire_) |
| **Grade**  | Le champ correspondant au grade. (_Obligatoire_) |
| **Divers**  |  Le champ "divers". (_Obligatoire_) |
| **Avec image**  |  true pour faire apparaitre une image associée à la fiche |
| **Image**  |  Le champ contenant l'image. Doit être de type **"Attachments"**. L'image retenue est la première image trouvée dans ce champ. Si aucune image n'est trouvée, elle sera remplacée par une icône générique |
| **Couleur du fond, du nom, du grade, du champ divers**  |  Les couleurs à utiliser |

<img src="./src/images/2.png" width="800px" height="400px" />

